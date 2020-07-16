# JDBC Note

Content

- Connection

## Connection

### Close Connection

Should I close a Connection obtained from a DataSource manually?

A connection obtained from a connection pool should be used exactly the same as a normal connection. The JDBC 4.2 specification (section 11.1) says about pooling:

> **When an application is finished using a connection, it closes the logical connection using the method `Connection.close`**. This closes the logical connection but does not close the physical connection. Instead, the physical connection is returned to the pool so that it can be reused.
>
> Connection pooling is completely transparent to the client: A client obtains a pooled connection and **uses it just the same way it** obtains and **uses a non pooled connection**.

## References

[Should I close a Connection obtained from a DataSource manually?](https://stackoverflow.com/questions/23957390/should-i-close-a-connection-obtained-from-a-datasource-manually)