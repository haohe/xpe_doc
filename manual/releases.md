## XPE releases

We provide releases in a special repository and as a valued customer, you should be able to sync to this repository and get the latest releases.

### Repository structure

```
  releases
     -xpe          : the latest xpe_dist_{build number}.jar 
       -modules    : latest xpe modules
      -xpe_ms      : the latest xpe_ms_dist_{build number}.jar
       -modules    : latest xpe ms modules
     -tools
       -printLog   : the print log tool jar file
       -dbtools    : the db tool jar file
```

### How to upgrade XPE

1. stop XPE
2. One normally puts the xpe jar file under the lib dir of the XPE dir, if that is the case, just create a new symbolic link to the latest jar file
3. start XPE

### How to upgrade XPE modules

1. make sure XPE is running
2. copy the corresponding xar file to the deploy directory under the XPE home directory.
3. check the xpe.log and ensure that the new module is deployed correctly
4. stop and start XPE again to ensure all applications to use the latest module.  If you do not restart XPE, only the newly deployed application will use the latest module.  

 ### How to upgrade XPE MS

1. stop XPE MS
2. One normally puts the xpe_ms jar file under the lib dir of the XPE MS dir, if that is the case, just create a new symbolic link to the latest jar file
3. start XPE MS


### How to upgrade XPE MS modules

1. make sure XPE MS is running
2. copy the corresponding xar file to the modules directory under the XPE home directory.
3. check the xpe_ms.log and ensure that the new module is deployed correctly
4. stop and start XPE MS again to ensure all applications to use the latest module.  


      
       
