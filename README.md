# ProxySocket

ProxySocket is an implementation of the SOCKS4/SOCKS5/HTTPS CONNECT protocol in C# originally developed by [mentalis.org](http://www.mentalis.org/soft/class.qpx?id=9)

It supports username/password authentication and asynchronous calls

It inherits from a standard Socket class

Example usage:

```C#
// create a new ProxySocket
ProxySocket s = new ProxySocket(AddressFamily.InterNetwork, SocketType.Stream, ProtocolType.Tcp);
// set the proxy settings
s.ProxyEndPoint = new IPEndPoint(IPAddress.Parse("10.0.0.5"), 1080);
s.ProxyUser = "username";
s.ProxyPass = "password";
s.ProxyType = ProxyTypes.Socks5;	// if you set this to ProxyTypes.None, 
									// the ProxySocket will act as a normal Socket
// connect to the remote server
// (note that the proxy server will resolve the domain name for us)
s.Connect("www.mentalis.org", 80);
// send an HTTP request
s.Send(Encoding.ASCII.GetBytes("GET / HTTP/1.0\r\nHost: www.mentalis.org\r\n\r\n"));
// read the HTTP reply
int recv = 0;
byte [] buffer = new byte[1024];
recv = s.Receive(buffer);
while (recv > 0) {
	Console.Write(Encoding.ASCII.GetString(buffer, 0, recv));
	recv = s.Receive(buffer);
}
// wait until the user presses enter
Console.WriteLine("Press enter to continue...");
Console.ReadLine();
```