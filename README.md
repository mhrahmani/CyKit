 <img src="http://www.blueskynet.org/edu/emotkit2.png" width="200px" height="200px"></img>
EmotKit 1.0
===========
for Python 2.7.6 (Windows x86)

<img src="http://www.blueskynet.org/edu/emotiv.jpg" width=50% height=50% ></img>

Description
-----------

EmotKit 1.0 is specifically for Python development in Windows to 

give access to the raw data stream from the Emotiv EPOC headset.  
Note that this will not give you processed data. 


EmotKit Dependancies
--------------------

* pywinusb 
* pycrypto
* gevent 
* greenlet
* pygame (Only required if you want to use render.py
   which shows the EEG graph)
* ctypes (Only required for mouse_control.py)

Python Usage
------------

  Code:
  
    import emotiv
    import gevent

    if __name__ == "__main__":
      headset = emotiv.Emotiv()    
      gevent.spawn(headset.setup)
      gevent.sleep(1)
      try:
        while True:
          packet = headset.dequeue()
          # packet.byteCode --- See Byte Codes below. 
          print packet.gyroX, packet.gyroY
          gevent.sleep(0)
      except KeyboardInterrupt:
        headset.close()
      finally:
        headset.close()


Direct links for Windows(x86) Dependancies
------------------------------------------

* pywinusb - https://pypi.python.org/packages/source/p/pywinusb/pywinusb-0.2.9.zip
* pycrypto - http://www.voidspace.org.uk/downloads/pycrypto26/pycrypto-2.6.win32-py2.7.exe
* gevent   - www.lfd.uci.edu/~gohlke/pythonlibs/#gevent ->  gevent‑1.0.1.win32‑py2.7.exe
* greenlet -  https://pypi.python.org/packages/2.7/g/greenlet/greenlet-0.4.2.win32-py2.7.exe#md5=0ea8f5a14f8554919e1a136bc042d76c
* Pygame(optional) - http://pygame.org/ftp/pygame-1.9.1.win32-py2.7.msi
* Ctypes(optional) - http://downloads.sourceforge.net/project/ctypes/gccxml/2008-08-12/gccxml-0.9.0-win32-x86.exe
 
Installation Instructions
-------------------------

* Install Python 2.6.7 https://www.python.org/ftp/python/2.7.6/python-2.7.6.msi


Extract pywinusb to any folder,  and copy 

                                       \pywinusb 
                                       
                                  from \pywinusb-0.2.9 to
                                        
                                 Drive:\Python27\Lib\site-packages
                                       
* Install following dependancies:

 gevent 1.0.1

 greenlet 0.4.2
 
 pycrypto 2.6
 
  
  Install to python2.7.6 folder.

Navigate to the Python directory extracted from EmotKit-master.zip

C:\Python27\Python.exe C:\EmotKit-master\Python\example.py

If your Emotiv USB dongle is not connected it will throw several errors ending with:



                                       AttributeError: 'Emotiv' object has no attribute 'device'


Connect the Emotiv dongle and run again, and it should begin streaming you data.

Note: Python 3 requires some cosmetic updates to work properly. 
   

Notes 
-----
If you are using the Python library and a research headset you may

have to change the type in emotiv.py's setupCrypto function. 

Raspberry Pi is not required, just plug dongle into USB.


Byte Codes
----------

This is a list of the byte names you can receive through packet.
( For example print packet.AF4 )

<img src="http://www.blueskynet.org/edu/emotivContacts.png"></img>

Sensor Bits {
  
  F3
  FC5
  AF3
  F7
  T7
  P7
  O1
  O2
  P8
  T8
  F8
  AF4
  FC6
  F4
  
}

Gyro Bits {

  GYRO_X
  GYRO_Y

}

Other {

   INTERPOLATED
   COUNTER
   BATTERY
   RESERVED
   ETE1
   ETE2
   ETE3

}

Server Support
==============

Added ability to stream the data to a TCP connection.


Updated a cleaner streaming server, with no EEG/graph.

Type RunStream.bat

    (runs: Python.exe stream.py)


Type RunServer.bat

     (runs: Python.exe streamdata.py)

It will pause waiting for a connection. 
When a connection is made, the server will stream
the data to the client. The EEG render graph also
displays 

Type RunClient.bat

    (runs: Python.exe client.py)

Once the server is running, run this and it will establish
a connection and print the data.  Sending first the ByteCode
of which sensor it is, and then trailing will be the data
from the sensor.

Note, the batch files adjust the PATH to run Python27, useful
if you have multiple versions of Python.  Adjust as necessary.


mIRC Support
=============

Added a mIRC script that will connect to the TCP server and display
in a simple graph the activity of the sensors. Will update later.

In Command Prompt type RunStream.bat

     (runs: Python.exe stream.py)

In mIRC "Status Window" type 

     /load -rs emotClient.mrc
     /load -rs emotSignal.mrc

This loads scripts to the remotes. Alternatives ALT+R and load manually.
then in "Status Window" type. 

emotClient connects to the socket server and breaks large packets into smaller ones.
emotSignal receives the smaller packets and handles displaying results.

     /eClient

<img src='http://www.blueskynet.org/edu/emotivmirc.png' width=80% height=80%></img>

Update to emotClient.mrc  had to revamp the script to read chunks of packet data.
Now uses Signal event to handle the chunks as lines of data.  There is an obvious
increase in resolution as its no longer missing packet data.

* Fixed stream.py skipping some packet data.

<img src='http://www.blueskynet.org/edu/emotivIRC1.png' width=80% height=80%></img>

* Fixed emotSignal.mrc displaying data incorrectly.

It turns out I was displaying the data values incorrectly.
I had been splitting the data into two segments thinking one
was quality and one was data, when they were both data.

as you can see in the image below, the dips indicate a subtle movement
such as blinking.
<br>
Special thanks to Bill Schumacher for fixing the Render.py

<img src='http://blueskynet.org/edu/emotKit1.png' width=80% height=80%></img>


Credits & Original Code
=======================

* Cody Brocious (http://github.com/daeken)
* Kyle Machulis (http://github.com/qdot)

Contributions by

* Severin Lemaignan - Base C Library and mcrypt functionality
* Sharif Olorin  (http://github.com/fractalcat) - hidapi support
* Bill Schumacher (http://github.com/bschumacher) - Fixed the Python library
* Bryan Bishop and others in #hplusroadmap on Freenode.


