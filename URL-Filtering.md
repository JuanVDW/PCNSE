# URL Filtering

### URL Filtering Security profiles
* Added to security policies that are set to 'allow'
* Applied to all packets over the life of a session
* Items are logged under:
    * Monitor > Logs
    * URL Category in the logs show which category the site falls under
    * The actions of 'Alert', 'Block', 'Continue' and 'Override' will generate a log entry
    * Filtering logs can be done with (URL contains 'facebook') to search for all entries with users going to facebook
* Rules can be created to block access to specific websites, or website categories
* A default profile is included to be used 'out of the box'
* A custom profile can be created based on your companies internal security policies
* A URL profile can be configured to take specific actions per each category
* If User-ID is configured, you can enable under the 'User Credential Detection' tab to log the user information to the logfiles
* To create a new custom URL Category, go to: Objects > Custom Objects > URL Category > Add
* Entries are case sensative, and subdomain considerations should be checked
    * www.ebay.com will not block cdn.ebay.com in a block list
    * *.ebay.com would block all ebay subdomains
* Allow list and block lists can be used to add sites you don't want users to access
* Actions available under the block list include:
    * Block: block access, access attempt is logged, and a response page is given to the user notifying them the site is blocked
    * Continue: a response page is presented, asking the user to confirm they want to proceed. Item is logged as 'block-continue' when the continue page is presented, and changed to 'continue' if the user proceed to the page.
    * Override: will prompt for an administrator page to override a URL block. Used for administrators and others that need a way to bypass blocks to some pages when needed.
    * Alert: allows the user to proceed without interruption, and generates an alert in the URL log
* Custom HTML pages can be created and uploaded to the PAN firewall
* Custom HTML block pages are limited to 16kb
* User's name will be displayed on the page if UserID is enabled; otherwise the IP will be displayed.
* If Continue or Override is used, a 15 minute timer is set to allow access to that category.
    * Timer can be changed at: Device > setup > content-id > URL Filtering
    * Admin Password can be changed at : Device > Setup > Content ID > URL Admin Override
    * Only one override password is allowed.
    * An SSL/TLS profile can be used to specify a certificate to secure the connection to the firewall if Admin override is set to 'Redirect'
    * Transparent mode can be used make block pages look to originate from the blocked website
    * Redirect will send the request to the specified IP. This IP must be an L3 interface on the firewall.
* Safe Search can be selected under Objects > Security Profiles > URL Filtering > (profile name) under the URL Filtering Tab
* This is based on the browser's safe search setting
* Log Container Page Only can be selected in this same section
* Only the name of the page will be logged if Log Container Page is selected(helps with log containment and size)
* Both SafeSearch and Log Container are both recommended settings by PAN for best practice
* Not-Resolved URL's include sites that are not in the local cache and could not contact the PAN Cloud to check the category
    * Recommend to set to 'alert'
    * Use the CLI command `show url-cloud status` to check cloud lookup service; should say 'connected'. If not connected, troubleshoot connectivity to this site (may need a service route installed)
* The local URL Seed Database locally on the firewall is based on the region the FW is installed, but doesn't contain all URL's that PAN has categorized as it would be too large. Local contains most common accessed, and others are checked 'on-demand' as not-resolved to the PAN cloud DB.
* If a URL is miscategorized by PAN, a request can be submitted to ask it to be recategorized. This is done under Monitor > Logs > URL Filtering - click on the entry you want to submit, click the 'request categorization change' under details. Fill out all information including comments, these are human reviewed and are generally responded to in 24-48 hours.
* A category check can be done in 2 ways:
    * By going to 'urlfiltering.paloaltonetworks.com and putting in the URL. You can also submit a category change here
    * By going under Objects > Security Profiles > URL Filtering > Add - click 'Check URL Category Link'

### Attaching URL Filtering Profiles
* URL Filtering can be added into Security Profile Groups with other security profiles such as AV, Vuln, File Block and Data filtering
* Either Individual or groups can be assigned to a Security Policy rules. This is dependant on your deployment and corporate security policy
* In the Security Policy, select the URL Profile or group you have created that you want to apply to the policy
* Reminder that only 'allow' policies evaluate URL policies. Polices set to deny or block traffic will do just that.
