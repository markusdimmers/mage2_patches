# Project MZENTRALE Magento Patches
Patches for Magento Core created by Mzentrale


## Installation
1. Optional Install - Module for Magento Patches
   bin/composer require cweagans/composer-patches
2. Add in composer.json following part:
    ```
    "extra": {
    "magento-force": "override",
    "patches": {
      "module to patch": {
        "Description": "path or url to patch file"
      }
    },
    ```

    ### Example
    ```
    "extra": {
     "magento-force": "override",
     "patches": {
       "magento/framework": {
         "MAGETWO:24902 - Mail non ASCII issue": "https://bitbucket.org/mzteam/magento-patches/src/master/composer/mage233/MAGTWO_24902_mail_issue.diff"
       }
     },
    ```

## Create a Patch

1. Easiest way to create a custom patch for magento 2.

[https://www.classyllama.com/blog/create-apply-patches-magento-2]

### Example Steps for magento/framework
```
#cd to the package that you're going to patch, as the paths in the patch need to be relative to the Composer package
cd vendor/magento/framework
git init .
git add -A .
git commit -m "Adding files to create diff"
#Make changes to the vendor/magento/framework/Mail/Template/TransportBuilder.php file and then proceed
git add -A .
git commit -m "Commit explaining the changes contained in the patch"
git format-patch -1 HEAD
#Rename the new patch
#Get rid of the temp Git repo
rm -rf .git
```
### Patch File Example
```
From 5099df742a699697fd271457b71b2727213ab632 Mon Sep 17 00:00:00 2001
From: Markus Dimmers <m.dimmers@mzentrale.de>
Date: Tue, 14 Jan 2020 15:51:51 +0100
Subject: [PATCH] mail_patch

---
 Mail/Template/TransportBuilder.php | 1 +
 1 file changed, 1 insertion(+)

diff --git a/Mail/Template/TransportBuilder.php b/Mail/Template/TransportBuilder.php
index 8e298db..3d41bbb 100644
--- a/Mail/Template/TransportBuilder.php
+++ b/Mail/Template/TransportBuilder.php
@@ -402,6 +402,7 @@ class TransportBuilder
             (string)$template->getSubject(),
             ENT_QUOTES
         );
+        $this->messageData['encoding'] = $mimePart->getCharset();
         $this->message = $this->emailMessageInterfaceFactory->create($this->messageData);

         return $this;
--
2.23.0
```
