# http协议

---

[like-curl](https://github.com/realcpf/like-curl/)

## 请求体
 > 这里的换行是有实际意义的\r\n
 ```text
 GET / HTTP/1.1
 Host: localhost:8080
 who: realcpf

 ```

 ```java
public static void main(String[] args) throws IOException {
    Socket client = new Socket();
    InetSocketAddress address = new InetSocketAddress("localhost",8000);
    client.connect(address,1000);
    String req = "GET /book/ HTTP/1.1\r\n" + 
                "Host: localhost:8000\r\n\r\n";
    try(PrintWriter printWriter = new PrintWriter(client.getOutputStream(),true)){
        printWriter.println(req);
        String info = null;
        try(BufferedReader bReader = new BufferedReader(new InputStreamReader(client.getInputStream(),"UTF-8"))){
            while ((info = bReader.readLine())!=null) {
                System.out.println(info);
            }
        }
    }
}
 ```

## alive with golang
 ```go
 func main() {
	conn, err := net.DialTimeout("tcp", "localhost:8080", time.Second)
	if err != nil {
		fmt.Println(err)
	}
	defer conn.Close()
	req := "GET / HTTP/1.1\r\n" +
		"Host: localhost:8080\r\n" +
		"who: realcpf\r\n" +
		"\r\n"
	conn.Write([]byte(req))
	// 在没有 SetReadDeadline 的情况下 会自动断开，并不会报出超时异常，
	// 会打印响应信息
	buf := make([]byte, 1024)
	_, err = conn.Read(buf)
	if err != nil {
		fmt.Println(err)
	}
	fmt.Printf("%s", string(buf))
 }

 ```
 > 没有SetReadDeadline 不会断开,不会打印响应信息
 ```go
	 var buf bytes.Buffer
	 len, err := io.Copy(&buf, conn)
	 if err != nil {
	 	fmt.Println(err)
	 }
	 fmt.Printf("len%d:%s", len, buf.String())
 ```

 > 没有SetReadDeadline 不会断开,会打印响应信息
 ```go
	buf := make([]byte, 1024)
	len := 0
	for {
		fmt.Printf("%d %s", len, string(buf))
		n, err := conn.Read(buf[len:])
		if n > 0 {
			fmt.Printf("n:%d\n", n)
			len += n
		}
		if err != nil {
			if err != io.EOF {
				fmt.Println(err)
				break
			}
			break
		}
	}
 ```
 > 没有SetReadDeadline 不会断开,不会打印响应信息
 ```go
    //

 	buf1, err := ioutil.ReadAll(conn)

	if err != nil {
		fmt.Println(err)
	}
    
	fmt.Println(string(buf1))
 ```