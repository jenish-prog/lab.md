# lab.md
import java.io.*;
import java.net.URL;
import java.nio.file.Files;
import java.nio.file.Paths;

public class Download {
    public static void main(String[] args) {
        try {
            String fileName = "digital_image_processing.jpg";
            String link = "http://tutorialspoint.com/java_dip/images/" + fileName;
            
            URL url = new URL(link);
            Files.copy(url.openStream(), Paths.get(fileName));
            System.out.println("Download complete.");
        } catch (Exception e) {
            System.out.println("Error: " + e.getMessage());
        }
    }
}
import java.io.*;
import java.net.*;

public class Server {
    public static void main(String[] args) {
        try (ServerSocket server = new ServerSocket(4000)) {
            System.out.println("Waiting for client...");
            Socket client = server.accept();
            System.out.println("Client connected");

            BufferedReader in = new BufferedReader(new InputStreamReader(client.getInputStream()));
            PrintWriter out = new PrintWriter(client.getOutputStream(), true);

            System.out.println("Received: " + in.readLine());
            out.println("Hello from server!");
        } catch (IOException e) {
            System.out.println("Server error: " + e.getMessage());
        }
    }
}


import java.io.*;
import java.net.*;

public class Client {
    public static void main(String[] args) {
        try (Socket socket = new Socket("localhost", 4000)) {
            System.out.println("Connected to server");

            PrintWriter out = new PrintWriter(socket.getOutputStream(), true);
            BufferedReader in = new BufferedReader(new InputStreamReader(socket.getInputStream()));

            out.println("Hi from client");
            System.out.println("Server says: " + in.readLine());
        } catch (IOException e) {
            System.out.println("Client error: " + e.getMessage());
        }
    }
}
import java.io.*;
import java.net.*;

public class tcpchatserver {
    public static void main(String[] args) throws IOException {
        try (ServerSocket server = new ServerSocket(4000);
             Socket client = server.accept();
             BufferedReader in = new BufferedReader(new InputStreamReader(client.getInputStream()));
             PrintWriter out = new PrintWriter(client.getOutputStream(), true);
             BufferedReader user = new BufferedReader(new InputStreamReader(System.in))) {
            String msg;
            while ((msg = in.readLine()) != null && !msg.equals("end")) {
                System.out.println("Client: " + msg);
                System.out.print("Server: ");
                msg = user.readLine();
                if (msg == null || msg.equals("end")) break;
                out.println(msg);
            }
        }
    }
}


import java.io.*;
import java.net.*;

public class tcpchatclient {
    public static void main(String[] args) throws IOException {
        try (Socket socket = new Socket("localhost", 4000);
             BufferedReader in = new BufferedReader(new InputStreamReader(socket.getInputStream()));
             PrintWriter out = new PrintWriter(socket.getOutputStream(), true);
             BufferedReader user = new BufferedReader(new InputStreamReader(System.in))) {
            String msg;
            while (true) {
                System.out.print("Client: ");
                msg = user.readLine();
                if (msg == null || msg.equals("end")) break;
                out.println(msg);
                if ((msg = in.readLine()) == null) break;
                System.out.println("Server: " + msg);
            }
        }
    }
}
import java.net.*;

class dnsserver {
    public static void main(String[] args) throws Exception {
        String[] h = {"yahoo.com", "gmail.com", "cricinfo.com", "facebook.com"};
        String[] i = {"68.180.206.184", "209.85.148.19", "80.168.92.140", "69.63.189.16"};
        try (DatagramSocket s = new DatagramSocket(53)) {
            while (true) {
                byte[] b = new byte[512];
                DatagramPacket p = new DatagramPacket(b, 512);
                s.receive(p);
                String n = new String(b, 0, p.getLength()).trim();
                int x = -1;
                for (int j = 0; j < h.length; j++) if (h[j].equalsIgnoreCase(n)) x = j;
                b = (x >= 0 ? i[x] : "NF").getBytes();
                s.send(new DatagramPacket(b, b.length, p.getAddress(), p.getPort()));
            }
        }
    }
}


import java.net.*;

class dnsclient {
    public static void main(String[] args) throws Exception {
        try (DatagramSocket s = new DatagramSocket()) {
            byte[] d = new java.util.Scanner(System.in).nextLine().getBytes();
            s.send(new DatagramPacket(d, d.length, args.length == 0 ? InetAddress.getLocalHost() : InetAddress.getByName(args[0]), 53));
            byte[] b = new byte[512];
            DatagramPacket r = new DatagramPacket(b, 512);
            s.receive(r);
            System.out.println(new String(b, 0, r.getLength()));
        }
    }
}
