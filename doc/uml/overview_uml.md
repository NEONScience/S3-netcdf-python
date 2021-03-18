@startuml

package "S3netCDF4" as s3netcdf {
    together {
        class s3Dataset #dodgerblue {
            -s3_groups : Dictionary<s3Group>
            -s3_dimensions : Dictionary<s3Dimension>
            -s3_variables : Dictionary<s3Variable>
            -nc_dataset : Dataset
            -cfa_dataset : CFADataset
            -file_manager : FileManager
        }

        class s3Group #deepskyblue {
            -s3_dimensions : Dictionary<s3Dimension>
            -s3_variables : Dictionary<s3Variable>
            -nc_grp : Group
            -cfa_grp : CFAGroup
        }

        class s3Dimension #darkturquoise {
            +cfa_dim : CFADimension
            +nc_dim  : Dimension
        }

        class s3Variable #cyan {
            +cfa_var : CFAVariable
            +cfa_dim : CFADimension
            -file_manager : FileManager
        }
        s3Dataset "1" *-- s3Variable
        s3Dataset "1" *-- s3Group
        s3Dataset "1" *-- s3Dimension

        s3Group "1" *-- s3Variable
        s3Group "1" *-- s3Dimension
    }

    package "Managers" as managers #oldlace {
        package "ConfigManager" as config_manager #whitesmoke{
            class Config #tomato {

            }
        }

        package "ConnectionManager" as connection_manager #seashell {
            class ConnectionPool #lightgray {
                -connection_pool : Dictionary<Connection>
            }
            class Connection #gray {
            }
        }
        ConnectionPool "1" *-- Connection

        package "FileManager" as file_manager #snow {
            class FileObject #tan {

            }

            class OpenFileRecord #goldenrod{
                +file_object : FileObject
                +data_object : Dataset
            }

            class OpenArrayRecord #goldenrod{
            }

            class FileManager #gold{
                -open_files : Dictionary<OpenFileRecord>
                -open_arrays: List<OpenArrayRecord>
                -config : Config
                +request_file()
                +request_array()

            }
        }
        OpenFileRecord "1" *-- "1" FileObject
        FileManager "1" *-- OpenArrayRecord
        FileManager "1" *-- OpenFileRecord
    }
}

package netCDF4 #linen {
    together {
        class Dataset #dodgerblue {
            +groups : Dictionary<Group>
            +dimensions : Dictionary<Dimension>
            +variables : Dictionary<Variable>
        }

        class Group #deepskyblue {
            +dimensions : Dictionary<Dimension>
            +variables : Dictionary<Variable>
        }

        class Dimension #darkturquoise {

        }

        class Variable #cyan {

        }
        Dataset "1" *-- Group
        Dataset "1" *-- Variable
        Dataset "1" *-- Dimension

        Group "1" *-- Variable
        Group "1" *-- Dimension
    }
}

package "CFA" as cfa #linen {
    together {
        class CFADataset #dodgerblue {
            +cfa_groups : Dictionary<CFAGroups>
        }
        class CFAGroup #deepskyblue {
            +cfa_dims : Dictionary<CFADimension>
            +cfa_vars : Dictionary<CFAVariable>
        }
        class CFAVariable #cyan {
            +nc_partition_group : Group
        }
        class CFADimension #darkturquoise {

        }
    }
    CFAGroup --* "1" CFADataset
    CFADimension --* "1" CFAGroup
    CFAVariable --* "1" CFAGroup

    package Parsers #lemonchiffon{
        class CFAParser #gainsboro {

        }
        class CFAnetCDFParser #gray {

        }
        CFAnetCDFParser <-- CFAParser
    }
}

package "Backends" as backends #lightyellow {
    class BufferedIOBase #orangered {

    }

    class s3FileObject #orange {
        -connection_pool : ConnectionPool
        -config : Config
    }

    class s3aioFileObject #orange {
        -connection_pool : ConnectionPool
        -config : Config
    }
    BufferedIOBase <-- s3FileObject
}

FileManager -- Config

s3aioFileObject -- ConnectionPool
s3FileObject -- ConnectionPool
Config -- s3aioFileObject  
Config -- s3FileObject

s3aioFileObject -- FileManager
s3FileObject -- FileManager

'Group "1" *-- "1" CFAVariable
'Dataset "1" *-- "1" OpenFileRecord

s3Dataset "1" *-- "1" Dataset  
s3Group  "1" *-- "1" Group
s3Dimension "1" *-- "1" Dimension
s3Variable  "1" *-- "1" Variable

s3Dataset "1" *-- "1" CFADataset
s3Dimension "1" *-- "1" CFADimension
s3Group "1" *-- "1" CFAGroup
s3Variable "1" *-- "1" CFAVariable

s3Dataset -- FileManager
's3Variable -- FileManager


@enduml
