# oai_redirection

The changement will describe for making OAI IMSI-CATCHER or redirection

Have a look on this tutorial for installation of OAI: 
* https://open-cells.com/index.php/2019/09/22/all-in-one-openairinterface/    

Have a look on this tutorial for doing the OAI redirection : 
* https://icepng.github.io/2020/09/23/RRCConnectionRelease/


PATCH is in the file asn1_msg.c as : https://github.com/SitrakaResearchAndPOC/oai_redirection/blob/main/asn1_msg_diff.txt

# REJECTION PROCESS AT OAI-CN : 

* all rejection message is definead at :  
src/nas/api/network/as_message.h


* For changing tracking_area_update_reject : (by default EMM_CAUSE_IMPLICITLY_DETACHED)  
mousepad src/nas/emm/TrackingAreaUpdate.c  
  
  
seach and change this code :   
rc = emm_proc_tracking_area_update_reject (ue_id, EMM_CAUSE_IMPLICITLY_DETACHED);  

NB : No need to change if we don't want DoS  


* For changing service_attach_reject :   
mousepad src/nas/nas_proc.c   
search DIAMETER_AUTHENTICATION_DATA_UNAVAILABLE (before s6a_error)  

IF UE is not in database, it will send this error so change with code reject  
all code is at : src/nas/api/network/as_message.h  
For example : NAS_CAUSE_NO_SUITABLE_CELLS_IN_TRACKING_AREA  if we want to make IMSI-CATCHER
  
  
This error code will be re-apply as attach_reject->emmcause on :  
mousepad src/nas/emm/msg/AttachReject.c 
