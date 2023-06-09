Download Link: https://assignmentchef.com/product/solved-cs352-program-4-a-file-uploader
<br>
<span style="font-size: 2.61792em; letter-spacing: -1px;">Program Description</span>

You will write both the <strong>Client</strong> (GUI) and the <strong>Server</strong> (console program) for a Microsoft Word document <strong>uploader</strong>. The Client will upload MS Word documents (in bytes stream) to the Server using port number <strong>5520.</strong>  The Server only handles one Client at a time. Therefore, you will NOT be using Java Thread. That is, you will NOT have a ServerThread class in the Server program. Your Server class must service the upload after it gets the connection and won’t service another client until the current one is over. This avoids having to deal with two clients uploading a file with the same name at the same time. Your programs must not crash under any situation. You must try-catch everything and print out appropriate error messages identifying the specific exceptions.

<strong>The Client  </strong>v The Client sends <strong>bytes         of         data</strong> to the Server in the following sequence:

<ul>

 <li>Begin with the <strong>file name</strong> followed by an ASCII null ‘<strong></strong><strong>0</strong>‘; the file name must be JUST the name, NOT the full path name.</li>

 <li>Next, the <strong>size</strong> of the selected file, as a string, also terminated with an ASCII null ‘<strong></strong><strong>0</strong>‘.</li>

 <li>Finally, the <strong>contents</strong> of the file.</li>

</ul>

For example, if the file <strong>ABC.doc</strong> were of size <strong>3125</strong> bytes, and the first byte in the file was ASCII ‘X’, the sequence of the byte stream sent form the Client to the socket output stream would be: <strong>                             ‘A’ ‘B’        ‘C’        ‘.’         ‘D’       ‘O’       ‘C’        ‘</strong><strong>0</strong><strong>‘      ‘3’        ‘1’        ‘2’        ‘5’        ‘</strong><strong>0</strong><strong>‘      ‘X’                                .           .           .</strong>

Note that these are ASCII bytes, NOT characters, since in Java, a “character” occupies two bytes whereas a “byte” only occupies one byte.

<ul>

 <li>After the Server has received and saved the file, the Server sends back the single ASCII byte: ‘<strong>@</strong>‘ and then disconnects from that client.</li>

 <li>The Client disconnects when it gets back the ‘<strong>@</strong>‘. If the client gets back a character other than ‘<strong>@</strong>‘, it still disconnects but prints an error message.</li>

 <li>You MUST have a GUI for your client program. For example, if you are using JFrame, you must have the following components:

  <ul>

   <li>A JFileChooser. You can set the fileFilter property to custom code:</li>

  </ul></li>

</ul>

<strong>FileNameExtensionFilter filter = new FileNameExtensionFilter( </strong>

<strong>                   “MSWord”, “doc”, “docx”); </strong>

<strong>chooser.setFileFilter(filter);</strong>

<ul>

 <li>On top of the Client program, import <strong>swing.JFileChooser.*, </strong>or add JFileChooser to the Swing designer.</li>

 <li>Use <strong>getSelectedFile()</strong> method to get the File object, so you know the file name, file size and the file path; <strong>getName(), getPath(),</strong> and <strong>length()</strong> methods of <strong>File </strong>will be very useful.</li>

 <li>A read-only JTextArea and proper JLabel to display all output messages. DO NOT print anything with System.out. Use the append() method of JTextArea to display all output. Be sure to print descriptive messages for ALL steps involving the Client/Server connection and the file transfer.</li>

 <li>Have a “Connect and Upload” JButton; when the user clicks the button, the Client will attempt to connect to the Server and upload the file selected. A sample GUI is shown as follows.</li>

</ul>




<ul>

 <li>You can consider having the following methods in the Client program.</li>

</ul>

/**

<ul>

 <li>This method takes a String s (either a file name or a file size,) as a</li>

 <li>parameter, turns String s into a sequence of bytes ( byte[] ) by calling</li>

 <li>getBytes() method, and sends the sequence of bytes to the Server. A null * character ‘ ’ is sent to the Server right after the byte sequence.</li>

</ul>

<h2>*/ private void sendNullTerminatedString(String s) {}</h2>

/**

<ul>

 <li>This method takes a full-path file name, decomposes the file into smaller</li>

 <li>chunks (each with 1024 bytes), and sends the chunks one by one to the</li>

 <li>Server (loop until all bytes are sent.) A null character ‘ ’ is sent to * the Server right after the whole file is sent.</li>

</ul>

*/ <strong>private void sendFile(String fullPathFileName){} </strong>

<h1>The     Server</h1>

<ul>

 <li>The Server must be a stand-alone console program and handle only one client connection at a time. There will only be a Server class – NO ServerThread class.</li>

 <li>In the <strong>run()</strong> method of the Server, create a new <strong>ServerSocket</strong> listening on port <strong>5520,</strong> run an infinite loop, waits for a connection, goes through the steps below and save the file in the same folder with your Server program. When done with a client, the Server waits for the next connection.

  <ul>

   <li>Get the null-terminated file name</li>

   <li>Get the null-terminated file size</li>

   <li>Get the file contents</li>

   <li>Send the character “@” back to the Client if the transfer is successful</li>

  </ul></li>

 <li>You MUST have a try-catch inside the infinite loop of the server, so that if an exception is thrown due to a bad exchange with a Client, the Server will recover and wait for the next connection. The server should never crash! v The Server must print out status messages so all communications can be followed at all times. For example, display “waiting for connection…” when the Server is ready and listening, etc. All messages should be displayed to the standard output (out.println().)</li>

 <li>When a connection is made, you can print out when and who, for example:</li>

</ul>

Server running…

Waiting for connection….

Got a connection: Sat Sep 27 15:54:06 CDT 2014

Connected to: /127.0.0.1 Port: 1261

Got file name: prog4.docx File size: 106527

Got the file.

Waiting for connection….

<ul>

 <li>You will be doing low-level byte I/O – sending and receiving bytes and/or arrays of bytes. This is similar to program 2 in which you transferred files through the HTTP.</li>

 <li>You can consider having the following methods in the Server program:</li>

</ul>

/**

<ul>

 <li>This method reads the bytes (terminated by ‘ ’) sent from the Client, one</li>

 <li>byte at a time and turns the bytes into a String.</li>

 <li>Set up a loop to repeatedly read bytes until a ‘ ’ is reached.</li>

</ul>

<h2>*/ private String getNullTerminatedString(){}</h2>

/**

<ul>

 <li>This method takes an output file name and its file size, reads the binary</li>

 <li>data (in a 1024-byte chunk) sent from the Client, and writes to the output</li>

 <li>file a chunk at a time.</li>

 <li>Use the FileOutputStream class to write bytes to a binary file * Set up a loop to repeatedly read and write chunks.</li>

</ul>

*/

<h2>private void getFile(String filename, long size){}</h2>

v I will have my Sever running on constance.cs.rutgers.edu at port 5520 for you to test your Client program before your Server program is done.

<h2>Program Grading</h2>

<table width="578">

 <tbody>

  <tr>

   <td width="372"><strong>Exceptions/Violations </strong></td>

   <td width="98"><strong>Each Offense </strong></td>

   <td width="108"><strong>Max Off </strong></td>

  </tr>

  <tr>

   <td width="372">Did not implement the client, or the client doesn’t run</td>

   <td width="98">15</td>

   <td width="108">15</td>

  </tr>

  <tr>

   <td width="372">Did not implement the server, or the server doesn’t run</td>

   <td width="98">10</td>

   <td width="108">10</td>

  </tr>

  <tr>

   <td width="372">Files are not transferred correctly</td>

   <td width="98">7</td>

   <td width="108">7</td>

  </tr>

  <tr>

   <td width="372">Did not use a GUI for the client program</td>

   <td width="98">5</td>

   <td width="108">5</td>

  </tr>

  <tr>

   <td width="372">Client malfunction, or not using 1024-byte chunks, or missing the required GUI components</td>

   <td width="98">1</td>

   <td width="108">4</td>

  </tr>

  <tr>

   <td width="372">Improper exception handling</td>

   <td width="98">1</td>

   <td width="108">2</td>

  </tr>

  <tr>

   <td width="372">Improper message output on Server/Client</td>

   <td width="98">1</td>

   <td width="108">3</td>

  </tr>

 </tbody>

</table>

<strong> </strong>