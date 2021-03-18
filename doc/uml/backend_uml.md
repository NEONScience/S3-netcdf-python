@startuml
left to right direction
package "Backends" as backends #lightyellow {
    class BufferedIOBase #orangered {

    }

    class s3FileObject #orange {
        -connection_pool : ConnectionPool
        -config : Config

        +__init__()           
        +__enter__ / __aenter__()
        +__exit__ / __aexit__()
        +connect()
        +close()
        +detach()
        +read()
        +read1()           
        +readinto()
        +readinto1()
        +readable()               
        +readline()
        +readlines()
        +write()
        +writeable()
        +writelines()
        +flush()
        +truncate()
        +seek()
        +seekable()
        +tell()
        +fileno()   
        +glob()
    }
    BufferedIOBase <-- s3FileObject
}

@enduml
