	Building a database requires understanding what pieces of information are relevant to the database. These pieces of information are called #attributes .
- _Attributes_ make up the columns in a database and describe the name and data type of the data that belongs in that column.
- [For more information](./sql)

## Database Management Systems

This data can be manipulated and controlled using a #DBMS (Database Management System).
- A _DBMS_ is software that manages and controls the data in a database.

Information systems that utilize a _DBMS_ to manage information are comprised of two other components: data and application software.
- _DBMS_ are also known as a #database-engine or a #back-end 
- In regards to the data being managed, they have the following responsibilities:
	- Data Definition: providing a way to define and build the database
	- Data Manipulation: providing a way to insert and update data in the database
	- Query Execution: answering questions about the data in the database
	- Data Integrity: ensuring that data stored is well formed
	- Data Security: enforcing restrictions about who is able to access what data
	- Data Portability: providing a means for backup and restore
	- Data Recovery: protecting data from loss in the case of a catastrophic failure in hardware of software
	- Provenance: logging capabilities to provide an audit trail for data changes
	- Performance: providing a means to tune and optimize operation
	- Multiuser Concurrency: supporting the activities of many users at the same time
	- Automatic Processing: providing a way to define rules to execute business logic (e.g. stored procedures and triggers)

Details of data storage in regards to _DBMS_ is irrelevant. It is only relevant in regards to performance.

The final component in a database approach to building software is the front-end aspect. #application-software is what the user interacts with and uses to communicate with the _DBMS_.

#sql is the structured language used to query data from a _DBMS_. _SQL_ is the medium for specifying what information is retrieved, updated, or created through explicit instructions.

Restrictions on what the user can perform within a _DBMS_ are employed through specifying users with explicit roles/descriptions of what they are eligible to perform. In full-stack development, the front-end application would have its own authentication that enables specific functionality. 


## Early Forms of System Development

### Mainframe Architecture

#mainframes were the earliest implementations of the database approach to system development. All three aspects of the database approach co-existed on the compute device:
- DBMS
- Data
- Application

![[Pasted image 20240831165152.png]]

Users were granted access through terminals.
- Referred to as _dumb terminals_
- Only provided a display and input peripherals to the _mainframe_

__Terminal emulation programs__ allow more powerful personal computers to fill the role of a _terminal_.

The network is involved only to connect the terminals to the mainframe. 
- Inputs from input peripherals are sent from the terminal to the _application software_  
- Results are sent back from the _application software_ to the terminals that are connected via the network.
- This approach is effective in locations where fast inexpensive network connections are unavailable.

Limitations of system performance in this sense of system development are caused by processing power and disk speed.

| Advantages of Mainframe Architecture                          | Disadvantages of Mainframe Architecture        |
| ------------------------------------------------------------- | ---------------------------------------------- |
| Large quantity of concurrent users                            | Does not take advantage of local compute power |
| Able to handle large numbers of concurrent users              | Hardware is relatively expensive               |
| Suitable for slow network environments                        | Character-based interface                      |
| Centralized, monolithic architecture makes maintenance simple |                                                |


### Desktop Architecture

Similar to mainframes as all three components as the application, DBMS, and data all exist on one machine.

![[Pasted image 20240831164330.png]]

However, it is limited to a single user and does not require a network connection.

The most popular desktop database is __Microsoft Access__. database files are stored as _.accdb_ files. 
- Interacting with this database is done through application software.
	- The application software is augmented with custom code or _macros_ that are attached to the forms developers create.
- __Microsoft Jet Engine__ is the DBMS in this case.
	- Installed as a part of Access.
	- Non-graphical

Simple information systems using database technology include such things as customer lists or inventory records.

| Advantages of Desktop Architecture | Disadvantages of Desktop Architecture       |
| ---------------------------------- | ------------------------------------------- |
| Inexpensive to acquire             | Only available to a single user at a time   |
| Easy to configure                  | Only available on single computer at a time |
| Requires few resources to operate  | May have limits on data stored              |

### File-Sever Architecture

Begins as Desktop Architecture but is expanded to support multiple concurrent users.
- Connected via the network

![[Pasted image 20240831164317.png]]

The separation of the DBMS from the data introduces a substantial bottleneck as network speeds are slower than disk speeds.
- This becomes even worse if more users are sharing internet traffic
- Querying the database also becomes slower if large amounts of data are being returned to the user.

DBMS in this instance must prevent multiple users from manipulating he same data simultaneously.

The main function of a file server is to enable multiple users to access the stored files and free storage space for the file repository.

These servers are popular as a central storage place for company files that are relevant for individual users or that can be shared by multiple users.

This architecture requires data backups that exist on the file server and apply for all users.

| Advantages of File-Server Architecture    | Disadvantages of File-Server Architecture                                              |
| ----------------------------------------- | -------------------------------------------------------------------------------------- |
| Inexpensive to acquire                    | Only viable for a very limited amount of users (below 10 likely)                       |
| Easy to configure                         | Query performance declines as data increases.                                          |
| Low-Cost option for mult-user concurrency | Multiple installations of both application and DBMS can lead to version control issues |


### Client/Server Architecure

