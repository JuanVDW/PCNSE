# Wildfire

### Wildfire Concepts
* When a file receives a file:
    * It will check to see if it is signed by trusted signer
    * If there is not a signature, it creates a hash of the file to check if it has already been sent to Wildfire
        * If not already submitted, it will check if it is below the maximum file size configured to be uploaded to WF
        * If exceeded max size, it is allowed through the firewall
        * If under max size, it is uploaded and checked with Wildfire, and the response is sent to the firewall
    * The Types of verdicts assigned to files scanned by Wildfire include:
        * Benign: Found to be safe and pose no risk
        * Greyware: No security threat but may display obtrusive behavior; adware, spyware, browser helper objects
        * Malware: The file contains a malicious payload; viruses, worms, trojans, rootkits, botnets and remote access tools
        * Phishing: Scans links in emails to determine if the site is a site to phish for credentials or other personal data
    * File attachments and URL in emails are also scanned and will be categorized in one of the options above
* When files and URL's are submitted to Wildfire, new signatures are generated and are available for download within 24-48 hours as content updates
* Two types of Wildfire subscription service:
    * Standard Subscription:
        * All systems running panOS 4.0+ can access Wildfire standard subscription service (as an XP or Win7 VM)
        * Includes Windows PE Analysis: EXE, DLL, SCR, FON, etc
        * AV signature delivered daily dynamic content updates (requires Threat prevention license)
        * Automatic file submission
    * Wildfire Licensed Service get standard features plus:
        * Additional file types scanned, including MSOffice files, PDF, JAR, CLASS, SWF, SWC, APK, Mach-O, DMG, and PKG
        * Wildfire signature files updated every 5 minutes
        * API File submission
        * Wildfire private cloud appliance: WF-500
* Wildfire Private Cloud
    * WF-500 is a private cloud Win7 64-bit image based Wildfire private system hosted on your network
    * Locally analyzes files forwarded from the FW or from the PAN XML API
    * Signatures can be generated locally. Benign and Greyware never leave the network
    * You have the option to forward malware to the wildfire cloud for signature generation
    * Signatures updates every 5 minutes
    * Supports XML API
    * Does not support Phishing; all positive matches are classified as 'malware'
    * Content updates can be installed manually or automatically
* Hybrid Cloud
    * Combines local and cloud solutions. WF-500 can analyze sensitive files locally, and less sensitive files can be uploaded to Wildfire for analysis.