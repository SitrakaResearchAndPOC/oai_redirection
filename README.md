# Schematic description
* Classic flow
<img src="https://github.com/SitrakaResearchAndPOC/oai_redirection/blob/main/schematic_classicflow.JPG" width="450px" align="center">
* IMSI-Catcher for non programmer without modification but with denied of service
<img src="https://github.com/SitrakaResearchAndPOC/oai_redirection/blob/main/schematic_imsicatcherdos.JPG" width="450x" align="center">
* IMSI-Catcher for programmer with modification but without denied of service
<img src="https://github.com/SitrakaResearchAndPOC/oai_redirection/blob/main/schematic_imsicatcher.JPG" width="450px" align="center">
* IMSI-Catcher for programmer with modification but with denied of service and redirection
<img src="https://github.com/SitrakaResearchAndPOC/oai_redirection/blob/main/schematic_imsicatcherdosredirection.JPG" width="750px" align="center">

# oai_redirection
The changement will describe for making OAI IMSI-CATCHER or redirection </br>
Have a look on this tutorial for installation of OAI: </br>
* https://open-cells.com/index.php/2019/09/22/all-in-one-openairinterface/    

Have a look on this tutorial for doing the OAI redirection : </br>
* https://icepng.github.io/2020/09/23/RRCConnectionRelease/
</br> </br>
PATCH is in the file asn1_msg.c as : https://github.com/SitrakaResearchAndPOC/oai_redirection/blob/main/asn1_msg_diff.txt

# REJECTION PROCESS AT OAI-CN : 

* all rejection message is definead at :  
```
gedit src/nas/api/network/as_message.h
```

* For changing tracking_area_update_reject : (by default EMM_CAUSE_IMPLICITLY_DETACHED coded as 10)
```
gedit src/nas/emm/TrackingAreaUpdate.c  
```  
  
* search and change this code :
```
rc = emm_proc_tracking_area_update_reject (ue_id, EMM_CAUSE_IMPLICITLY_DETACHED);  
```
NB : 
* WITH JUST COLLECTOR AND JAMMER -> Denied of service afer imsi-catcher ( ILLEGAL_UE coded 3) for default service_attach_reject
* NO NEED DENIDED OF SERVICE , MODIFY service_attach_reject as  NAS_CAUSE_NO_SUITABLE_CELLS_IN_TRACKING_AREA (ONLY ACTIVE IMSI-CATCHER)

* For changing service_attach_reject :   
```
gedit src/nas/nas_proc.c   
```
search DIAMETER_AUTHENTICATION_DATA_UNAVAILABLE (before s6a_error)  </br></br>

IF UE is not in database, it will send this error so change with code reject  </br>
all code is at : src/nas/api/network/as_message.h  </br>
For example : NAS_CAUSE_NO_SUITABLE_CELLS_IN_TRACKING_AREA (coded 15) if we want to make IMSI-CATCHER </br>
  
  
This error code will be re-apply as attach_reject->emmcause on :  
```
gedit src/nas/emm/msg/AttachReject.c 
```


# Modification for redirection at openairinterface5g
```
find -name asn1_msg.c 
```
```
find -name ASN1_files/EUTRA-RRC-Definitions-a20.asn
```
```
find -name LTE_RedirectedCarrierInfo*
```
```
find -name LTE_RedirectedCarrierInfo.c
```
```
find -name LTE_RedirectedCarrierInfo.h
```
```
find -name LTE_CarrierFreqsGERAN*
```
```
find -name LTE_CarrierFreqsGERAN.h
```
For building oai for redirection, use release 14
```
./build_oai -I --eNB -x --install-system-files -w USRP -r Rel14
```
