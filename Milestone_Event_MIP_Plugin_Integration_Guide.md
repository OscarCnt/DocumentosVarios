---
title: Milestone Event MIP Plugin Integration Guide
slug: iris-plus-integrations/milestone-event-mip-plugin-integration-guide
createdAt: 2024-08-20T10:12:57.576Z
updatedAt: 2025-09-30T09:46:15.289Z
---

## &#x20;Supported Milestone XProtect Editions & Versions Supported IRIS+™ products

IRIS+ integration aligns with your Milestone XProtect deployment to support your needs. It supports using Milestone XProtect Express, Milestone XProtect Corporate, and Milestone Open Network Bridge deployment. &#x20;

Check[ ](https://docs.irisity.com/iris-plus-integrations#1mztU) for supported Milestone XProtect versions.&#x20;

## System Compatibility

The Milestone Event MIP plugin is only compatible with Microsoft Windows Server versions. Installation on Windows 10 or Windows 11 Desktop versions is not supported.

## Support for Milestone Interconnect

Milestone Interconnect is a central surveillance hub integrating smaller, remote Milestone XProtect installations. Thus, it serves as a central site for video stream access. Integrated with the IRIS+ analytics solution, you can receive a centralized view of the IRIS+ alarms from dispersed Milestone sites (individually integrated into IRIS+) in your Milestone Interconnect smart client tool. For more details, refer to [**Milestone Documentation**](https://doc.milestonesys.com/latest/en-US/feature_flags/ff_interconnectedproducts/sc_miinterconnect.htm?Highlight=INTERCONNECT)

## Time Synchronisation

IRIS+™ provides time synchronization options supported as part of the Milestone XProtect integration. Detected events in IRIS+™ can be synced to one of the following:&#x20;

- Detected events synced to the IRIS+™ Edge™ Device time – syncs Milestone XProtect cameras with the IRIS+™ Edge™ Device timing. Relevant for Milestone XProtect when Open Network Bridge is not deployed.

- Recommended: Detected events synced to the video stream time (when available) – supported for Milestone Open Network Bridge deployed cameras. This method provides the best sync between IRIS+™ and Milestone XProtect. &#x20;

Note: For more information on the Milestone Open Network Bridge solution, refer to this [**documentation**](https://doc.milestonesys.com/latest/en-US/portal/htm/chapter-page-onvif.htm)

# Map Cameras

## Before you start

The IRIS+™ Milestone XProtect integration targets cameras (or the camera’s video streams) defined in Milestone XProtect and maps them to IRIS+™. This mapping procedure means that the camera must be defined in IRIS+™ and that the Camera’s ID often named GUID (Globally Unique Identifier) in Milestone XProtect is required to identify and link the camera in IRIS+™. In this way, the MIP Plugin can identify the source cameras and their detected events. &#x20;

This procedure also supports Milestone Interconnect. When retrieving the camera’s GUID, make sure to do so for the relevant Milestone Interconnect version and tools. &#x20;

## Retrieve Camera ID

### XProtect Professional

1. Open "systeminfo.xml" via a web browser here: *http\://localhost//rcserver/systeminfo.xml&#x20;*
2. Search for the camera name in the systeminfo.xml file  
3. The camera GUID is shown beneath the camera name:
4. Copy the details and keep them for use later in the IRIS+™ Portal&#x20;
5. Perform the setup for all cameras that you wish to connect to IRIS+™    

### XProtect Corporate, XProtect Professional+

The camera GUIDs are available in this file: C:\Program Files\Milestone\MIPPlugins\IRIS+EventMonitoring\MilestoneCameras.ini

IRIS+™**&#x20;Portal**

- Open the IRIS+™ Portal and find the relevant camera
- In the camera’s Settings tab, click the Edit button &#x20;
- In the External ID field, enter the camera ID you retrieved for this camera in the XProtect Management Client. This field is case-sensitive so ensure the camera ID is entered exactly as it was acquired from Milestone XProtect&#x20;

![](https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-Wh6g_ZWhGtbZqo3BWJHBg-20240730-141309.png)

- For Milestone Interconnect deployment, enter the Interconnect-specific camera ID in the External ID Field. If this is being added as additional support on top of the Milestone XProtect VMS, then separate the two entries with a comma&#x20;

:::hint{type="info"}
Multiple external IDs can be applied to a single camera; please ensure that you hit enter on your keyboard to submit multiple external IDs before saving.
:::

- Add the camera connection string in the "Video stream source" under Video Source, For Open Network Bridge deployment only, use the following URI format in the Video Stream Source field: &#x20;

  RTSP://\[username]:\[password]@\[hostname/IP]:\[Open Network Bridge RTSP port]/live/\[Milestone camera ID] &#x20;

  To learn more, watch this Milestone tutorial: [**https://www.youtube.com/watch?v=-LIRbga2LOk**](https://www.youtube.com/watch?v=-LIRbga2LOk)

  &#x20;
- Ensure the Sync time to stream toggle switch is enabled for Open Network Bridge deployment only. See image:

![](https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-QS6MUIAxY8RurNOY09bn1-20240730-143003.png)

You can integrate IRIS+™ with two different Milestone systems. You must add the second GUID to the cameras in IRIS+™ separated by a comma.

# Enable Milestone Integration in IRIS+™ Portal

## Before you start

This section describes how to create an IRIS+™ service account and token in the IRIS+™ Portal to later link to the Milestone XProtect Management Client. &#x20;

These steps assume that your IRIS+™ account has been set up and that folders, devices, and cameras have been configured. If that is not the case, first access IRIS+™ tutorials from the IRIS+™ Support hub to configure your account. &#x20;

In IRIS+™, a service account is required to later link the Milestone XProtect MIP Plugin to IRIS+™. The IRIS+™ service account provides a token, which is the identifier used to link the account to the MIP Plugin.&#x20;

If connecting multiple IRIS+™ accounts to a single Milestone VMS deployment, this procedure must be performed for each IRIS+™ account. &#x20;

### To generate an IRIS+™ Service Account token, perform the following: &#x20;

- Browse to your IRIS+™ account, Settings tab



![](https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-OaIctd9uyD-MkeiwaiPla-20240730-143617.png)

- Click the Users tab &#x20;

![](https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-wzfVpa9BVGm2nGKtpcVHY-20240730-143839.png)



- Click the Add button and select Create service account. &#x20;

![](https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-BampbT0BkxZ9-QvbtkWzj-20240730-144355.png)

::Image[]{src="https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-C4RpnLA145-YYWP0bRJwm-20240730-144153.png" signedSrc size="44" width="452" height="532" position="center" caption}

-
  Enter a Service Account name such as “Milestone Integration”. Enter a description (optional).&#x20;
- Click Create Service Account. &#x20;
- From the user's list, select the created Service Account user, click the arrow next to the name to expand the view, and select Get Token&#x20;

::Image[]{src="https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-77S0ldn21TIegZnYosnAU-20240730-145021.png" signedSrc size="88" width="872" height="322" position="center" caption}

- Define token expiration if required (by default the token never expires)

::Image[]{src="https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-114Hrpf5DDNKvt_weue12-20240730-145658.png" signedSrc size="56" width="523" height="363" position="center" caption}

- Click Get token and note the generated token (to be used in the Event Monitoring MIP plugin configuration):&#x20;

::Image[]{src="https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-ttz26S9CaNnL9_OJnz50K-20240730-145847.png" signedSrc size="56" width="526" height="341" position="center" caption}

- Close the window&#x20;

# Install and Configure IRIS+™ Event Monitoring MIP Plugin

The integration of IRIS+™ and Milestone XProtect, based on the Milestone Integration Platform (MIP), offers these benefits:&#x20;

- Simple to configure. It takes just a few steps to be able to receive events for any number of cameras and any number of analytics rules per camera &#x20;
- You can view past events, navigate to a video recording of a specific event, and view analytics tracking for that event &#x20;

Note: To receive IRIS+™ events in Milestone XProtect, make sure that all Milestone XProtect Event Server machines and/or Milestone XProtect Management Client machines have outbound TCP access to IRIS+ servers - \*.irisity.io:443 (Irisity hosted) or \*.irisplus.app:443 (On-Prem deployment)

## Download and Install IRIS+™ Event Monitoring MIP Plugin

**Before you start**

&#x20;Install the IRIS+™ Event Monitoring MIP Plugin on all PC hosting:  

- Milestone XProtect Event Server  
- Milestone Client (Management or Smart Client)  
- Milestone Interconnect server and management applications  

Note: If an earlier version of the Event Monitoring MIP Plugin is already installed, install the new version on top of it (i.e., upgrade).  

**To install the IRIS+™ Event Monitoring MIP Plugin (or upgrade from a previous version),  do the following: &#xA0;**

- Close the Milestone XProtect Management Client application. &#x20;
- Download the latest plugin version from  [**Available Downloads**](docId\:M2kgcn_L71j33O0fw92Mq). (1.0.0.143)
- Run the IRIS+™ Event Monitoring MIP Plugin install wizard. Follow the instructions until the installation is complete.

## Initialize IRIS+™ in Milestone XProtect Management Client

The integration supports the following methods:&#x20;

**IRIS+™ Core&#x20;**&#xD;
Send analytic events and events overlay from IRIS+™ Core to Milestone XProtect
Send health events from IRIS+™ Core to Milestone XProtect&#x20;

**IRIS+™ Edge™ Device**
Send analytics events from IRIS+™ Edge™ Device to Milestone XProtect. &#x20;
The live overlay is available in Milestone XProtect Smart Client.&#x20;

- When Core is available
  Events overlay is available in Milestone XProtect Smart Client&#x20;

- When Core is NOT available
  Events overlay is available in Milestone XProtect Smart Client for 4 seconds before the event time&#x20;

Notes:&#x20;
*1. The purpose of this option (core not available) is to enable continued support of events transfer to Milestone XProtect if the core is not available for short periods&#xA0;*

*2. If the Edge™ Device is restarted for any reason while the core is not available, the connection to the cameras is lost. The core must be available to renew a connection&#xA0;*

*3. Schedules for rules are updated in the Edge™ Device every 24 hours. If the core is not available for more than 24 hours, the Edge™ Device stops generating events&#xA0;*

### Requirements for IRIS+™ Edge™ Device integration:&#x20;

- Run Milestone XProtect Smart Client as administrator 
- In the Properties / Logon tab of the Milestone XProtect Event Server service, select an administrator user  
- External ID changes 
- Changes in camera External ID can be made in Milestone XProtect and/or in IRIS+™. Do the following: 

  - Change in Milestone XProtect: restart Milestone XProtect Event Server service to review the change immediately or wait up to 15 minutes and the change will take effect 
  - Change in IRIS+™: restart Milestone XProtect Event Server service to review the change 
  - Close and re-open the Milestone XProtect Smart Client
  - Following a change of integration type (IRIS+™ Core / IRIS+™ Edge™ Device), restart the Milestone XProtect  Event Server service and Close and re-open the Milestone XProtect Smart Client 

### To initialize the integration, do the following:&#x20;

1\. Open the Milestone XProtect Management Client 

2\. In the navigation tree, expand MIP Plug-ins → IRIS+ Event Monitoring 

::Image[]{src="https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-eEV-lJlWVreYiwOzIc55--20240801-114238.png" signedSrc size="38" width="299" height="807" position="center" caption}

3\. Right-click the IRIS+ Event Monitoring server and select Add New. 

*Note*: If connecting multiple IRIS+™ accounts to a single Milestone VMS deployment, perform this step and the following steps per IRIS+™ account. 

4\. The following IRIS+ Event Monitoring Information window appears: 

![](https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-H1SQ_vvYi07Qu0yNJfKzU-20240801-114513.png)

5\. Select the required integration method

**For Core integration**

- Populate the following fields in the window

  - Name: Change as per your preference
  - Server URL:&#x20;
    - Irisity hosted deployment: [**https://api.irisplus.ai**](https://api.irisity.io) 
    - Customer-hosted (on-premise) deployment: [**https://api.irisplus.app**](https://api.irisplus.app)
  - Token: paste the IRIS+ token you saved as part of your service account

- Mark the events you want to receive from IRIS+:&#x20;

  - Analytics events 
  - Health events 
    - All health events 
    - User-defined health event 


*Note*: refer to the chapter Configuring Health Events for a detailed description of how to configure Health events, alarms, and actions in Milestone XProtect client

- XProtect event Server IP Address: &#x20;
  enter the relevant IP address. The remaining fields are not applicable.  



Example:&#x20;

![](https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-yANe5o5R64tEg1BeQCQ4d-20240801-115521.png)

**For Edge integration**
Populate only the **Name&#x20;**&#x66;ield in the IRIS+ Event Monitoring Information window:&#x20;

- XProtect event Server IP Address: enter the relevant IP address
    
- Events Integration Port: enter the relevant Port. Once entered the value of the Events Integration URI field is automatically updated. If you perform this configuration on the Milestone XProtect Event Server, keep the Events Integration URI value to be used in the Edge integration definition in IRIS+ (see next chapter)
    
- Metadata Integration Port: enter the relevant Port. Once entered the value of the Metadata Integration URI field is automatically updated. If you perform this configuration on the Milestone XProtect Event Server, keep the Metadata Integration URI value to be used in the Edge integration definition in IRIS+ (see next chapter)  

Example:

![](https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-42ffkJTRGCqHBbdmkCSB6-20240801-115759.png)

6\. If you are interested in sending event snapshots to Milestone XProtect, mark this checkbox

Example

![](https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-GRnDIwWsBHMnR1JeBvGzN-20240801-120119.png)

7\. Exit the screen and select Save when prompted&#x20;

8\. Ensure that Analytics Events are enabled in the XProtect Management Client by doing the following:&#x20;

- From the Tools tab at the top, select Options. The Options panel opens.  
- Select the Analytics Events tab and ensure that the Enabled field is checked

::Image[]{src="https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-_4qi2CFIUR9T7pgJJIu8W-20240801-120453.png" signedSrc size="80" width="643" height="529" position="center" caption}

# Enable Edge™ Device integration in IRIS+™

Perform the following actions when integrating IRIS+™ to Milestone XProtect using the Edge integration option &#x20;

The Integration Targets in IRIS+™ are the system-wide definitions required for IRIS+™ to integrate with Milestone XProtect. You will define the endpoints that can receive the events and metadata sent by IRIS+™. &#x20;

## Define Integration Targets in IRIS+™

To define the Integration Targets in IRIS+™, do the following:

- Log in to your IRIS+™ account.  
- From the top module bar, select Settings.  

![](https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-6P3BrOfC3-Mhb84FZ8_Be-20240801-121706.png)

- Select Integration Targets.  

![](https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-MAd41PvSCeNZ3LKUUuRWQ-20240801-121847.png)

- Click the Add button. The Integration Target screen appears.  

![](https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-rZvY9PMGa5kSpvJ0NU6mB-20240801-122101.png)

![](https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-VmbrTpwtMPXi9zG8-aE_r-20240801-122211.png)

- Define the following:  


**Events integration**

- Target name: Define a meaningful name such as Milestone Events&#x20;
- Ensure the toggle switch is Enabled&#x20;
- Type: Keep the default value HTTP
- Method: Keep the default value POST&#x20;
- URI: Enter the same URI you have defined in the MIP Plugin installed on the Milestone XProtect Event Server&#x20;
- Click Save. The integration Target is defined and is listed in the Integration Targets list

Example

![](https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-Du25OIPflAlFgQTFCAv9c-20240801-122559.png)

**Metadata integration**

- Target name: Define a meaningful name such as Milestone Overlay&#x20;
- Ensure the toggle switch is Enabled&#x20;
- Type: Set Type value to WS
- Method: Set Method value to GET
- URI: Enter the same URI you have defined in the MIP Plugin installed on the Milestone XProtect Event Server&#x20;
- Click Save. The integration Target is defined and is listed in the Integration Targets list

Example

![](https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-5j6vIjyMSv3bGofz7ISLR-20240801-122942.png)

&#x20;

## Define Edge™ Device Integrations in IRIS+™&#x20;

The Edge™ Device Integrations in IRIS+™ are the IRIS+™ Edge-specific definitions required for IRIS+™ to integrate with Milestone XProtect. Perform the following for each Edge™ Device you want to integrate with Milestone XProtect. &#x20;

To define the Edge™ Device Integration in IRIS+™, do the following: &#x20;

- Log in to your IRIS+™ account
- Select Physical View

![](https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-ThGbUNby8O5DJePwxn0Md-20240801-130513.png)

- Select the IRIS+™ Edge for which you want to define the integration
- Select Integrations&#x20;

![](https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-3geqgDsEcTiCA19JX8a5s-20240801-131249.png)

- Click Edit

![](https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-bIzuMj4aGmzgctO1TTlp6-20240801-131405.png)

- Enable Events Integration and select the relevant Integration Target you have previously defined (in this example – Milestone Events) &#x20;

![](https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-3pwEYPRQ2ci8qTDu0ZsE_-20240801-131533.png)

- Enable Metadata Integration and select the relevant Integration Target you have previously defined (in this example – Milestone Overlay) &#x20;

![](https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-PavBfeVHQtI-9SUWCQVnQ-20240801-131623.png)

1. Click Save

# Synchronize Milestone XProtect Management Server Time to IRIS+™ Edge Time&#x20;

### Before you start

IRIS+™ supports time sync to the IRIS+™ Edge™ Device time. The IRIS+™ Edge™ Device and the Milestone XProtect Management Server time definitions must therefore be synchronized. Verify that the NTP (Network Time protocols) are synchronized on both servers. &#x20;

Note: If your deployment is synced to the video stream time using the Milestone Open Network Bridge solution, disregard this section. &#x20;

# Enable Overlays

To enable overlays in Milestone XProtect Smart Client, do the following:  

- Open Milestone XProtect Smart Client  
- Select Settings 

::Image[]{src="https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-QURhnqF4-WFW_VaCQCiIh-20240801-132425.png" signedSrc size="46" width="341" height="281" position="center" caption}

- Select IRIS+™ Event Monitoring 
  Enable the following fields as required: 
  - Show Live Overlay  
  - Show Playback Overly 

![](https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-KeTm517srZJ_kKJ6XbLVp-20240801-132539.png)

# Configure Default IRIS+™ Event & Alarm in Milestone XProtect

The default configuration described in this section allows for every event sent from IRIS+™ to be reported as an alarm in Milestone XProtect Smart Client. &#x20;

The triggering flow is: &#x20;

::Image[]{src="https://api.archbee.com/api/optimize/JzHznijrWxPLJLDUaV5A1/GKULrESfspU1ZV1QgyXUQ_image.png" size="88" width="497" height="68" caption="Triggering flow" position="flex-start"}

## Define IRIS+™ XProtect Analytics Event&#x20;

To define an IRIS+™ XProtect analytics event, do the following: &#x20;

1\. From the XProtect Management Client site navigation tree, navigate to Rules and Events or in XProtect Management Application, Events, and Output and select Analytics Events. &#x20;

2\. Right-click Analytics Events and select Add New

::Image[]{src="https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-BNA7BWkyfwNtX6rs7L5dP-20240801-133305.png" signedSrc size="50" width="532" height="393" position="center" caption}





3\. In the Properties section, for the Name field, enter "IRIS+ Event"

::Image[]{src="https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-NHquq2KqgdEBeWtIeywrY-20240801-133625.png" signedSrc size="50" width="691" height="309" position="center" caption}





## Define IRIS+™ Alarm in Milestone XProtect&#x20;

To define IRIS+™ alarm in Milestone XProtect, do the following:

1. From the Site Navigation tree, expand Alarms and select Alarm Data Settings.&#x20;

2. Select the Alarm List Configuration tab.&#x20;

3.
   Ensure that at least the following are included in the selected columns:&#x20;
   •	Time&#x20;
   •	Source&#x20;
   •	Tag&#x20;
   •	Message&#x20;

::Image[]{src="https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-JSTr8Q71JWWarqvgeAzkt-20240801-134037.png" signedSrc size="90" width="784" height="653" position="center" caption}

4\. From the Site Navigation tree, expand Alarms and select Alarm Definitions&#x20;
5\. Right-click Alarm Definitions and click Add New.&#x20;


::Image[]{src="https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-yp5yPI7WYCeIB-Awbf4kJ-20240801-134307.png" signedSrc size="80" width="730" height="655" position="center" caption}

6\. In the Alarm Definition Information, enter the following:&#x20;

&#x20; •	Name: IRIS+ Alarm&#x20;
&#x20; •	Triggering event: choose&#x20;

1. Analytics Events in the first line followed by
2. IRIS+ Event in the second line


&#x20; •	Sources: click Select

1. in the Select Sources screen that opens, open the Servers  tab, choose All cameras, and Add it to the Selected list&#x20;


::Image[]{src="https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-SSNsp1GZ2YD9WIASCQ_YH-20240801-135650.png" signedSrc size="60" width="610" height="347" position="center" caption}

Example

![](https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-TOh-AsfHOXGsywH3oaie8-20240801-135742.png)




7\. Exit the screen and select Save when prompted.



## Restart Milestone XProtect Event Server service&#x20;

**Before you start&#x20;**&#xD;
For the configuration to take effect you must restart the Milestone XProtect Event Server service on the relevant PCs.&#x20;

::Image[]{src="https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-9gr0FlTp6hF_VVtLHWuPC-20240801-140007.png" signedSrc size="50" width="413" height="265" position="center" caption}






# Configure and View Alarms in Milestone XProtect Smart Client

**Before you start**
The following steps explain how to view the IRIS+™ alarms in the Milestone Xprotect Smart client application&#x20;

**To configure and view IRIS+™ Alarms in Milestone XProtect Smart Client, do the following:**

1. Open the Milestone XProtect Smart Client application&#x20;
   &#x20;
2. Define a view, as follows:&#x20;
   - Select the Live tab on the left-hand side of the application window 

   - Click the Setup button on the right-hand side of the application window:  

   - Define a new group using the New Group icon 

   - Right-click the newly created group name and define a new view, for example, (1 + 2\*); make sure to select a view broad enough to contain the alarms list. 
      
   - From System Overview, drag the Alarm List item to the broad part of your newly created view
     &#x20;
     *Note*: you can change the order of the Alarm List columns. It is recommended to move the Tag column to the right so that its value becomes visible since it contains an event description
       
   - In System Overview, expand the cameras list and drag the relevant cameras to the remaining views.

3. In the Alarms list, note the Tag column containing the analytics event description (e.g., ‘Vehicle moving in an area’). If the Tag column is unavailable, right-click the table header bar to add it. If you are still unable to add it in conjunction with Milestone XProtect Corporate, refer to Alarm Data Settings in XProtect Management Client described above. &#x20;

## Event overlay setup

To properly sync the overlay and alarm times, do the following:&#x20;

1. Open the Milestone XProtect Smart Client  
2. Select Settings 

::Image[]{src="https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-QURhnqF4-WFW_VaCQCiIh-20240801-132425.png" signedSrc size="46" width="341" height="281" position="center" caption}

1. Select Alarm Manager 
2. Set the value of the Start video playback to 8 seconds before the alarm.

![](https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-4_0BBWBNkuSST8c8FRQox-20240801-141858.png)

# Triggering Specific Actions with Milestone XProtect Corporate&#x20;

Before you start

This section explains how to handle more advanced scenarios for triggering an action when an event occurs. The capability is available in Milestone XProtect Corporate Edition and is achieved by linking the analytics event to the XProtect user-defined event and user-defined rule. &#x20;

The triggering flow is: &#x20;



![](https://api.archbee.com/api/optimize/JzHznijrWxPLJLDUaV5A1/xbwJEN9HcJBMe1yI5eLjA_image.png)

To configure IRIS+™ and Milestone XProtect Corporate for triggering actions, do the following:&#x20;

1. In the IRIS+™ portal, select the relevant camera and then select the relevant detection rule.

2. Define an External ID for the rule; it will be used in the Milestone XProtect configuration. &#x20;

![](https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-s1EpvzIrzleVUQm44G4pq-20240801-142836.png)

1. In the Milestone XProtect Management Client navigation tree, Select Analytics Events, right-click, and select Add New.&#x20;

2. Enter the name identical to the External ID defined in IRIS+™. In this example: code1.&#x20;

![](https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-unt6KB-DU8bocOR4i4K_B-20240801-143326.png)

1. In the Milestone XProtect Management Client Navigation tree, select User-defined Events, right-click, and select Add User-defined Event.&#x20;

2. Enter a name for the new User-Defined Event and save.&#x20;

![](https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-LP-pdQl6TSnPfrBn7MzfZ-20240801-143750.png)

3\. Add a new alarm that links between the Analytics Event and the User-defined Event. In the Milestone XProtect Management Client navigation tree, select Alarm Definitions, right-click, and select Add New. &#x20;


4\. In the Alarm Definition Information, enter the following: &#x20;

- **Name**: meaningful name&#x20;
- **Triggering event**: select the Analytics Events defined in the previous steps (code1) &#x20;
- **Sources**: click Select; in the Select Sources screen that opens, open the Servers tab, choose the camera/s, and Add them to the Selected list &#x20;
- **Events triggered by alarm**: Select the user-defined event defined in the previous steps (myEvent)

![](https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-EbycIWgF5mhsVrmjwzADv-20240801-150152.png)

5\. Finish the alarm definition exit the screen and select Save when prompted.&#x20;

6\. After completing the steps above, restart the Milestone XProtect Event Server service for the configuration to take effect

Example

![](https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-XiPTWGfd84to-J9Z0wbb8-20240801-150301.png)

# Configuring health events

This section explains how to configure Health events in Milestone XProtect. There are two options to receive Health events in Milestone:&#x20;

## Receive all health events

This option enables receiving all IRIS+™ health events into a single (generic) health event type in Milestone XProtect. Perform the following actions:&#x20;

1. Add a new Analytics Event in the Milestone XProtect Management Client. &#x20;

2. Set the name to IRIS+ Health Event.&#x20;

![](https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-IgZcD4jzt7IO4ZtBqDXVV-20240801-150520.png)

3\. Add a new Alarm, which will be triggered by the IRIS+ Health Event. &#x20;


4\. In the Alarm definition Information, enter the following:&#x20;


- Name: IRIS+ Health Alarm&#x20;
- Triggering event: choose Analytics Events in the upper list and IRIS+ Health Event in the lower list as shown&#x20;
- Sources: click Select; in the Select Sources window that opens, open the Servers tab, choose All cameras, and Add it to the Selected list&#x20;
- Exit the screen and select Save when prompted&#x20;

![](https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-L_LK0NNBGbxB14z_c1TA6-20240801-150850.png)

## Receive user-defined health events&#x20;

This option enables receiving specific IRIS+™ health events according to user preference. &#x20;

One or more of the following events can be set in Milestone XProtect Management Client and associated alarms will be displayed in Milestone XProtect Smart Client.&#x20;

1. Communication error  
2. Camera error  
3. Source error 
4. Unsupported URI
5. Unsupported video codec
   - Description - Unsupported video codec, change the source stream configuration
6. Unsupported video resolution 
   - Description - Unsupported video resolution, change the source stream configuration
7. Large time gap in the stream&#x20;
   - Description - Large time gap in the stream, check the source stream
8. Critical framerate
9. High framerate
10. Low framerate
11. Image blocked
12. Image dark
13. Image saturated
14. Suspended sensor
    - Description - Suspended (banned) sensor - when it is detached from the appliance, the sensor configuration and rules still exist, the sensor is not connected to any appliance
15. Disabled sensor
    - Description - The sensor is disabled by the user (or by Arm/Disarm command)
16. Inactive sensor
    - Description - The sensor is not active due to user action (enable/disable, attach/detach)
17. Sensor active error
    - Description - The sensor is enabled by the user, active, and in error state&#x20;
18. Sensor active warning
    - Description - The sensor is enabled by the user, active, and in a warning state&#x9;
19. ONVIF error 
    - Description - ONVIF error, contact Irisity customer support
20. ONVIF not reachable
    - Description - ONVIF host not reachable, check the address and user\password&#x9;
21. RTSP authentication error
    - Description - RTSP authentication error, check the user and password
22. RTSP not reachable
    - Description - RTSP host not reachable, check the host and port address and try toggling the multicast support setting 
23. RTSP stream issue&#x20;
    - &#x20;Description - RTSP stream issue, try opening with VLC player&#x9;
24. RTSP timeout
    - Description - RTSP timeout, try toggling the multicast support setting&#x20;
25. Clip error
    - Description - Failed to download clip, check the path
26. Insufficient Calibration
    - Description - Insufficient auto-calibration&#x20;

The description of the health event will be displayed in the Tag field of the Alarm List in the Milestone XProtect Smart Client. It will be one of the supported user-defined events listed above.&#x20;

The example below shows adding a user-defined Health event:&#x20;

- Add a new Analytics Event in the Milestone XProtect Management Client. The name must be set to one of the events listed above.&#x20;

![](https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-d4hfVU5i51gK6owncQd2c-20240801-151453.png)

- Add a new User-defined Event in the Milestone XProtect Management Client:&#x20;

![](https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-eBmaMcXJg0SjitmMAkqiQ-20240801-151622.png)

- Add a new Alarm, linking the Analytics Event and the User-defined Event.&#x20;

::Image[]{src="https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-k75kACi1wH4Qk1skHSLkq-20240801-151826.png" signedSrc size="64" width="436" height="176" position="center" caption}

- In the Alarm definition Information, enter the following (for this specific example): 
  - Name: Communication error Alarm  
  - Triggering event: choose Analytics Events in the upper list and Communication error in the lower list as shown 
  - Sources: click Select; in the Select Sources window that opens, open the Servers tab, choose All cameras, and Add it to the Selected list 
  - Events triggered by alarm: select the user-defined event added above - *Communication error User-Defined Event*
  - Exit the screen and select Save when prompted 

::Image[]{src="https://api.archbee.com/api/optimize/0qwUIPTrS6-vWLcHFHOFA-Izvc-c44glRjR9UIs-Zg4-20240801-152037.png" signedSrc size="90" width="548" height="622" position="center" caption}

- For both options:&#x20;
  - New health events are displayed in the Alarms list with New State Names:  
  - Closed health events are displayed in the Alarms list with Closed State Name:&#x20;
  - Double-click on a health event to view additional details for the event:&#x20;



![](https://api.archbee.com/api/optimize/JzHznijrWxPLJLDUaV5A1/q3SP8n4heluwD0ldf3lxB_image.png)

![](https://api.archbee.com/api/optimize/JzHznijrWxPLJLDUaV5A1/DxdozMe-TXLB8y5FOpddf_image.png)

# Display Event Snapshot in Milestone XProtect Smart Client

*Only applicable if Send event image is enabled in the rule configuration of IRIS+™ &#x20;*

If the Send event analytics snapshot is checked, IRIS+™ sends event snapshots to Milestone XProtect. 

- Core integration 

Receive event snapshot with an overlay from IRIS+™ and send to Milestone XProtect Event Server 

- Edge integration 

Receive event image and metadata from the IRIS+™ Edge and after drawing the overlay on the received image, send the snapshot to Milestone XProtect Event Server.

The snapshot is displayed on the right-hand side of the event playback:&#x20;

![](https://api.archbee.com/api/optimize/JzHznijrWxPLJLDUaV5A1/142Y4oTmCiNOCpnPCE7a5_image.png)

You can use the Default for camera title bar setting in the Milestone XProtect Smart Client to Show or Hide the camera’s title:&#x20;

![](https://api.archbee.com/api/optimize/JzHznijrWxPLJLDUaV5A1/lptXDk3F7-EuoXUFNYzBa_image.png)

For Grouping and Occupancy rule types, the overlay for the snapshot and playback is drawn using the “common” overlay which contains all relevant individual event objects. See the following example:

# Display IRIS+™ in Milestone XProtect Smart Client&#x20;

Once the IRIS+™ Event Monitoring MIP Plugin is installed, an IRIS+™ tab is available in the Milestone XProtect Smart Client. Once selected, a login window is displayed. Log in to IRIS+™ using your credentials. &#x20;

# Troubleshoot IRIS+™ Event Monitoring MIP Plugin Integration&#x20;

| **Problem**                                                                                                                                                                                                                                               | **Corrective Action**                                                                                                                                                                                                     |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Milestone XProtect Management Client<br />**IRIS+™ is not displayed under the MIP plugins node in Milestone XProtect Management Client**                                                                                                                  | Verify that the IRIS+™ Event Monitoring MIP plugin is installed                                                                                                                                                           |
| **Milestone XProtect Management Client**                                                                                                                                                                                                                  | Verify that the Milestone XProtect Event Server service is running                                                                                                                                                        |
| Milestone XProtect Smart Client:&#xA;There are no analytics alarms in Milestone XProtect Smart Client                                                                                                                                                     |                                                                                                                                                                                                                           |
| Milestone XProtect Smart Client:&#xA;**There is no metadata (or only partial metadata appears) when playing back recorded video in Smart Client**                                                                                                         | Click the \*\*Play \*\*button again in case it was not clicked the first time                                                                                                                                             |
| Milestone XProtect Smart Client:&#xA;There are no alarms in the Alarm List, the header is red, and it displays a message regarding user privileges in Milestone XProtect Smart Client                                                                     | Verify the user connected to the Milestone XProtect Smart Client has sufficient user privileges, as follows: In **XProtect Management Client**, check the properties of the user under **Advanced Configuration > Users** |
| Milestone XProtect Smart Client:&#xA;All the above actions did not help; you are still unable to view analytics events in Milestone XProtect. Follow the instructions under the Corrective Action column to the right, to obtain Milestone MIP log files. |                                                                                                                                                                                                                           |
| Milestone XProtect Smart Client:&#xA;There is no possibility of adding a Tag column to the Alarm List                                                                                                                                                     |                                                                                                                                                                                                                           |
| Milestone XProtect Smart Client:&#xA;An error occurs when opening the Milestone XProtect Smart Client on Windows Server 2008                                                                                                                              |                                                                                                                                                                                                                           |
| The recorded video is not synchronized with object metadata overlays                                                                                                                                                                                      | Set up the same NTP endpoint on Edge Device, cameras, and Milestone XProtect                                                                                                                                              |

# Contact Irisity Support

Customer Support: [**technicalsupport@irisity.com**](mailto\:technicalsupport@irisity.com)
