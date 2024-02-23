# Debugging Interface Write-up

Category: Hardware 

 CHALLENGE DESCRIPTION

We accessed the embedded device's asynchronous serial debugging interface while it was operational and captured some messages that were being transmitted over it. Can you decode them?

Download the task file, unzip it and view the `.sal` file.
                                                                                                                                                                                                                                
    ┌──(kali㉿kali)-[~/Downloads]
    └─$ unzip Debugging\ Interface.zip 
    Archive:  Debugging Interface.zip
    [Debugging Interface.zip] debugging_interface_signal.sal password: 
      inflating: debugging_interface_signal.sal  
                                                                                                                                  
    ┌──(kali㉿kali)-[~/Downloads]
    └─$ ls
    debugging_interface_signal.sal  
    'Debugging Interface.zip'     
                                                                                                                                  
    ┌──(kali㉿kali)-[~/Downloads]
    └─$ file debugging_interface_signal.sal 
    debugging_interface_signal.sal: Zip archive data, at least v2.0 to extract, compression method=deflate   

Read about `.sal` file and look for what we can open it with, we found `saleae` tool, download it for our system 

![image](https://github.com/zer00d4y/writeups/assets/128820441/0c66317e-b472-4163-9e87-49bac4666d91)

![image](https://github.com/zer00d4y/writeups/assets/128820441/ae8b32a2-3952-4313-9ea0-ffe7bb2e6705)

                                                                                                                                  
    ┌──(kali㉿kali)-[~/Downloads]
    └─$ chmod +x Logic-2.4.13-linux-x64.AppImage 
                                                                                                                                  
    ┌──(kali㉿kali)-[~/Downloads]
    └─$ ./Logic-2.4.13-linux-x64.AppImage --no-sandbox
    /tmp/.mount_Logic-2Ut2WC ~/Downloads
    ~/Downloads
    Environment
      Executable path: /tmp/.mount_Logic-2Ut2WC/Logic
      Executable directory: /tmp/.mount_Logic-2Ut2WC
      Original working directory: /home/kali/Downloads
      Current working directory: /tmp/.mount_Logic-2Ut2WC
      Process ID: 1223810
    Crash reporting enabled. Machine ID: 7b8573d0-a3be-4224-8c2b-d3094f112db7
    libva error: vaGetDriverNames() failed with unknown libva error
    [1223926:0223/004553.076082:ERROR:gpu_memory_buffer_support_x11.cc(44)] dri3 extension not supported.
    [1223926:0223/004615.747818:ERROR:gl_utils.cc(319)] [.WebGL-0x5608f89a70b0]GL Driver Message (OpenGL, Performance, GL_CLOSE_PATH_NV, High): GPU stall due to ReadPixels
    [1223926:0223/004615.808629:ERROR:gl_utils.cc(319)] [.WebGL-0x5608f89a70b0]GL Driver Message (OpenGL, Performance, GL_CLOSE_PATH_NV, High): GPU stall due to ReadPixels
    [1223926:0223/004615.841247:ERROR:gl_utils.cc(319)] [.WebGL-0x5608f89a70b0]GL Driver Message (OpenGL, Performance, GL_CLOSE_PATH_NV, High): GPU stall due to ReadPixels
    [1223926:0223/004615.856819:ERROR:gl_utils.cc(319)] [.WebGL-0x5608f89a70b0]GL Driver Message (OpenGL, Performance, GL_CLOSE_PATH_NV, High): GPU stall due to ReadPixels (this message will no longer repeat)
    
    (Logic:1223810): Gtk-WARNING **: 00:46:31.197: Failed to fetch network locations: Timeout was reached
    error submitting otlp telemetry request: FetchError: request to https://o11y.saleae.com/v1/logs failed, reason: socket hang up
        at ClientRequest.<anonymous> (/tmp/.mount_Logic-2Ut2WC/resources/app.asar/node_modules/node-fetch/lib/index.js:1491:11)
        at ClientRequest.emit (node:events:526:28)
        at TLSSocket.socketOnEnd (node:_http_client:466:9)
        at TLSSocket.emit (node:events:538:35)
        at endReadableNT (node:internal/streams/readable:1345:12)
        at process.processTicksAndRejections (node:internal/process/task_queues:83:21) {
      type: 'system',
      errno: 'ECONNRESET',
      code: 'ECONNRESET'
    }

![image](https://github.com/zer00d4y/writeups/assets/128820441/4ada59e6-d82f-41b6-bcee-1ee218598078)

Analyzers -> Async Serial

![image](https://github.com/zer00d4y/writeups/assets/128820441/b44b74c8-5db9-417d-8f11-f3481d56058d)

![image](https://github.com/zer00d4y/writeups/assets/128820441/92d9f873-9608-4d57-893b-ac387a4c0e8f)

Click save, and select console 

And here we see the flag!

![Снимок экрана 2024-02-23 122913](https://github.com/zer00d4y/writeups/assets/128820441/760e5f5a-0ff1-4488-b5b1-afaf6d76c011)





