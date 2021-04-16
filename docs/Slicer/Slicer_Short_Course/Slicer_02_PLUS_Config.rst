.. _Slicer_02_PLUS_Config:

===================================================
Slicer Tutorial #2: PLUS and the Configuration file
===================================================

--------

Overview
********

PLUS is a software package that allows Slicer to communicate with OptiTrack, a tracking system. It can be downloaded `here <https://plustoolkit.github.io/>`__. In this walkthrough, I have downloaded the Win32 package, to work with Motive version 2.1.

PLUS is an intermediary between your tracking software (e.g., OptiTrack) and your visualization software (e.g., Slicer), as shown in the following diagram:

.. figure:: 01_PLUS_Diagram.png

.. note::

  PLUS is designed to work with Windows operating systems. Although there are options to build the code on Macintosh and Linux operating systems, this procedure is more complicated; we will not be covering it in this walkthrough.


The Configuration File
**********************

Since PLUS serves as an intermediary, a **configuration file** (also called a **config file**) is needed to indicate certain specifications about your hardware and which devices you want to record from.

A sample config file can be found on PLUS applications user manual website, located `here <http://perk-software.cs.queensu.ca/plus/doc/nightly/user/Configuration.html>`__. For OptiTrack, click on the ``OptiTrack`` link on that page; you will see two sample configuration files, one for all of the rigid bodies saved into a Motive profile .xml file, and the other one with the rigid bodies identified separately. For the latter config file, we have the following template code which defines different **objects**; that is, different pieces of software and hardware:

::

    <PlusConfiguration version="2.7">
    <DataCollection StartupDelaySec="1.0">
      <DeviceSet
        Name="PlusServer: OptiTrack (Profile file and additional rigid body TRA files)"
        Description="Broadcasting tracking data through OpenIGTLink."
      />
      <Device
        Id="TrackerDevice"
        Type="OptiTrack"
        ToolReferenceFrame="Tracker" 
        Profile="OptiTrack/EmptyProfile.xml"
        AttachToRunningMotive="FALSE"
        MotiveDataDescriptionsUpdateTimeSec="1.0" >
        <DataSources>
          <DataSource Type="Tool" Id="Stylus" />
          <DataSource Type="Tool" Id="Reference" />
        </DataSources>
        <OutputChannels>
          <OutputChannel Id="TrackerStream">
          <DataSource Type="Tool" Id="Stylus" />
          <DataSource Type="Tool" Id="Reference" />
          </OutputChannel>
        </OutputChannels>
      </Device>
    </DataCollection>
    <PlusOpenIGTLinkServer
      MaxNumberOfIgtlMessagesToSend="1"
      MaxTimeSpentWithProcessingMs="50"
      ListeningPort="18944"
      SendValidTransformsOnly="TRUE"
      OutputChannelId="TrackerStream" >
      <DefaultClientInfo>
        <MessageTypes>
          <Message Type="TRANSFORM" />
        </MessageTypes>
        <TransformNames>
          <Transform Name="StylusToTracker" />
          <Transform Name="ReferenceToTracker" />
        </TransformNames>
      </DefaultClientInfo>
    </PlusOpenIGTLinkServer>

   </PlusConfiguration>
   
   
Each block of code is encapsulated in arrowheads, with left arrowheads (e.g., ``<``) indicating the beginning of a block of code, and a forward slash indicating the end of a block of code (e.g., ``</``). For example, the block of code that defines the ``DeviceSet`` object has the following **attributes**, or additional information about that particular object:

::

      <DeviceSet
      Name="PlusServer: OptiTrack (Profile file and additional rigid body TRA files)"
      Description="Broadcasting tracking data through OpenIGTLink."
      />
      
In this example, the ``DeviceSet`` has the attributes ``Name`` and ``Description`` which both give it a label and a brief explanation of what it is.


The Modified Configuration File
*******************************

We will modify the above template configuration file to match our setup. For example, if we create a folder called MyExp on our Desktop, we will then create a Motive profile called "MyProfile.xml", as well as two rigid-body objects: A stylus (i.e., a pointer that contains several tracking markers on its body), and a reference object (e.g., any object that remains stationary with regard to the stylus). We will also change the boolean ``AttachToRunningMotive`` to ``TRUE``, to allow real-time updates from OptiTrack as we view an object in Slicer.

Another block of code that we will need is a **Virtual Capture** device. After the first TrackerDevice that is listed, we will enter code for another one called VirtualCapture. All of these modifications can be seen in code below:

::

      <PlusConfiguration version="2.7">
      <DataCollection StartupDelaySec="1.0">
        <DeviceSet
          Name="PlusServer: OptiTrack (Profile file and additional rigid body TRA files)"
          Description="Broadcasting tracking data through OpenIGTLink."
        />
        <Device
          Id="TrackerDevice"
          Type="OptiTrack"
          ToolReferenceFrame="Tracker" 
          Profile="MyProfile.xml"
          AttachToRunningMotive="TRUE"
          MotiveDataDescriptionsUpdateTimeSec="1.0" >
          <DataSources>
            <DataSource Type="Tool" Id="Stylus" />
            <DataSource Type="Tool" Id="Reference" />
          </DataSources>
          <OutputChannels>
            <OutputChannel Id="TrackerStream">
            <DataSource Type="Tool" Id="Stylus" />
            <DataSource Type="Tool" Id="Reference" />
            </OutputChannel>
          </OutputChannels>
        </Device>
        <Device 
            Id="CaptureDevice" 
            Type="VirtualCapture"
            BaseFilename="RecordingTest.igs.mha"
            EnableCapturingOnStart="FALSE" >
            <InputChannels>
              <InputChannel Id="TrackerStream" />
            </InputChannels>
         </Device>
      </DataCollection>
      <PlusOpenIGTLinkServer
        MaxNumberOfIgtlMessagesToSend="1"
        MaxTimeSpentWithProcessingMs="50"
        ListeningPort="18944"
        SendValidTransformsOnly="TRUE"
        OutputChannelId="TrackerStream" >
        <DefaultClientInfo>
          <MessageTypes>
            <Message Type="TRANSFORM" />
          </MessageTypes>
          <TransformNames>
            <Transform Name="StylusToTracker" />
            <Transform Name="ReferenceToTracker" />
            <Transform Name="StylusToReference" />
          </TransformNames>
        </DefaultClientInfo>
      </PlusOpenIGTLinkServer>

     </PlusConfiguration>

Copy and save this code into a text file called ``MyConfig.txt``. We now turn to the Motive software to create the profile and rigid-body objects that are needed by the configuration file.
