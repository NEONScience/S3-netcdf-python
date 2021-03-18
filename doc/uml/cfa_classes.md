@startuml

left to right direction

package "CFA" as cfa #linen {
    together {
        class CFADataset #dodgerblue {
            +cfa_groups : Dictionary<CFAGroups>

            +createGroup()
            +getGroup()
            +renameGroup()
            +getName()
            +getGroups()
            +getMetadata()
            +getCFAVersion()
            +getFormat()
        }
        class CFAGroup #deepskyblue {
            +cfa_dims : Dictionary<CFADimension>
            +cfa_vars : Dictionary<CFAVariable>

            +createVariable()
            +getVariable()
            +getVariables()
            +renameVariable()
            +createDimension()
            +getDimension()
            +getDimensions()
            +renameDimension()
            +getMetadata()
            +getName()
            +setName()
            +getDataset()
        }
        class CFAVariable #cyan {
            +nc_partition_group : Group

            +getGroup()
            +getName()
            +getType()
            +getMetadata()
            +getDimensions()
            +getRole()
            +shape()
            +getPartitionMatrixShape()
            +getPartitionMatrixDimensions()
            +getPartitionValues()
            +getPartition()
            +writePartition()
            +writeInitialPartitionInfo()
        }
        class CFADimension #darkturquoise {
            +getName()
            +getMetadata()
            +getLen()
            +getAxisType()
            +setType()
            +getType()
        }

        class CFAPartition #mediumturquoise{
            +np.ndarray index
            +np.ndarray location
            +string ncvar
            +string file
            +string format
            +np.ndarray shape
        }
    }
    CFAGroup --* "1" CFADataset
    CFADimension --* "1" CFAGroup
    CFAVariable --* "1" CFAGroup
    CFAPartition -- CFAVariable

    package Parsers #lemonchiffon{
        class CFAParser #gainsboro {
            +read()
            +write()
            +is_file()
        }
        class CFAnetCDFParser #gray {
            +read()
            +write()
            +is_file()
        }
        CFAnetCDFParser <-- CFAParser
    }
    CFAnetCDFParser "1" -- "1" CFADataset
}

@enduml
