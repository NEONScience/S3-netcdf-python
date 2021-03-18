@startuml

left to right direction

package "FileManager" as file_manager #snow {
    class FileObject #tan {
        +read_from()
        +read()
        +close()
        +glob()
        +size()
        -bool : remote_system
        -bool : async_system
        -s3BackendObject : file_handle
        -string : mode
    }

    enum open_state_mapping {
        OPEN_NEW_IN_MEMORY
        OPEN_EXISTS_IN_MEMORY
        KNOWN_EXISTS_ON_STORAGE
        OPEN_NEW_ON_DISK
        OPEN_EXISTS_ON_DISK
        KNOWN_EXISTS_ON_DISK
        DOES_NOT_EXIST
    }

    class OpenFileRecord #goldenrod{
        +string : url
        +integer : size
        +file_object : FileObject
        +data_object : Dataset
        +float : last_accessed
        +open_state_mapping : open_state
        +string : open_mode
        +bool : lock
    }

    enum array_type_mapping {
        IN_MEMORY
        MEMMAP
    }

    class OpenArrayRecord #goldenrod{
        -integer : size
        -array_type_mapping : array_type
        -string : array_location
    }

    class FileManager #gold{
        -open_files : Dictionary<OpenFileRecord>
        -open_arrays: List<OpenArrayRecord>
        -config : Config

        +get_file_open_state()
        +request_file()
        +open_success()
        +free_file()
        +free_all_files()
        +request_array()
        +free_all_arrays()

    }
}
OpenFileRecord "1" *-- "1" FileObject
FileManager "1" *-- OpenArrayRecord
FileManager "1" *-- OpenFileRecord
array_type_mapping -- OpenArrayRecord
open_state_mapping -- OpenFileRecord


@enduml
