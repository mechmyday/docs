2.2.4 New Features and Changes
==============================

Security/Errata Notices
-----------------------

-  `pfSense-SA-15_07.webgui <https://www.pfsense.org/security/advisories/pfSense-SA-15_07.webgui.asc>`__:
   Multiple Stored XSS Vulnerabilities in the pfSense® WebGUI

   -  The complete list of affected pages and fields is listed in the
      linked SA.

-  `FreeBSD-SA-15:13.tcp <https://www.freebsd.org/security/advisories/FreeBSD-SA-15:13.tcp.asc>`__:
   Resource exhaustion due to sessions stuck in LAST_ACK state. Note
   this only applies to scenarios where ports listening on pfSense
   itself (not things passed through via NAT, routing or bridging) are
   opened to untrusted networks. This doesn't apply to the default
   configuration.
-  Note:
   `FreeBSD-SA-15:13.openssl <https://www.freebsd.org/security/advisories/FreeBSD-SA-15%3A12.openssl.asc>`__
   does not apply to pfSense. pfSense did not include a vulnerable
   version of OpenSSL, and thus was not vulnerable.

-  Further fixes for file corruption  in various cases during an unclean shut
   down (crash, power loss, etc.).
   `#4523 <https://redmine.pfsense.org/issues/4523>`__

   -  Fixed pw in FreeBSD to address passwd/group corruption
   -  Fixed config.xml writing to use fsync properly to avoid cases when
      it could end up empty.
      `#4803 <https://redmine.pfsense.org/issues/4803>`__
   -  Removed the 'sync' option from filesystems for new full installs
      and full upgrades now that the real fix is in place.
   -  Removed softupdates and journaling (AKA SU+J) from NanoBSD, they
      remain on full installs.
      `#4822 <https://redmine.pfsense.org/issues/4822>`__

-  The forcesync patch for
   `#2401 <https://redmine.pfsense.org/issues/2401>`__ is still
   considered harmful to the filesystem and has been kept out. As such,
   there may be some noticeable slowness with NanoBSD on certain slower
   disks, especially CF cards and to a lesser extent, SD cards. If this
   is a problem, the filesystem may be kept read-write on a permanent
   basis using the option on **Diagnostics > NanoBSD**. With the other
   above changes, risk is minimal. We advise replacing the affected
   CF/SD media by a new, faster card as soon as possible.
   `#4814 <https://redmine.pfsense.org/issues/4814>`__

-  Upgraded PHP to 5.5.27 to address CVE-2015-3152
   `#4832 <https://redmine.pfsense.org/issues/4832>`__

-  Lowered SSH LoginGraceTime from 2 minutes to 30 seconds to mitigate
   the impact of MaxAuthTries bypass bug. Note
   :doc:`Sshlockout </firewall/sshlockout>` will lock out offending IPs in all past,
   current and future versions.
   `#4875 <https://redmine.pfsense.org/issues/4875>`__

Certificates
------------

-  Changed the built-in certificate manager to specify keyUsage and
   extendedKeyUsage in certificates. Windows will now correctly function
   with IKEv2 using certificates from the built-in certificate manager
   without disabling EKU.
   `#4580 <https://redmine.pfsense.org/issues/4580>`__

   -  Note: This change applies only to new certificates, created on
      2.2.4 or newer, and the CN of the certificate must match the
      hostname or IP address to which clients connect.

-  Added authorityKeyIdentifier to CRLs generated by the built-in
   certificate manager. (strongSwan requires it to match.)
   `#4860 <https://redmine.pfsense.org/issues/4860>`__

IPsec
-----

-  Fixed non-GCM AES modes with AES-NI enabled.
   `#4791 <https://redmine.pfsense.org/issues/4791>`__
-  Fixed issues with keyid and some mobile IPsec identifiers.
   `#4811 <https://redmine.pfsense.org/issues/4811>`__
   `#4806 <https://redmine.pfsense.org/issues/4806>`__
-  Fixed includes so PHP shell session restartipsec script works.
-  Fix dashboard hardware crypto display where AES-NI is enabled.
   `#4809 <https://redmine.pfsense.org/issues/4809>`__
-  Fixed issues with IPsec with certificates/ASN1.DN.
   `#4792 <https://redmine.pfsense.org/issues/4792>`__
   `#4794 <https://redmine.pfsense.org/issues/4794>`__
-  Added code to write out CRLs from the built-in certificate manager
   for use by strongSwan.
-  Added option for enabling Strict CRL Checking (strictcrlpolicy in
   strongSwan config).
-  Fixed saving Advanced IPsec options before IPsec is enabled.
-  Changed LAN bypass to be from "LAN subnet" to "LAN subnet" rather
   than from "LAN subnet" to "LAN address" to allow it to work for VIPs
   on the interface.
-  Remove "Auto" key exchange option, and change upgraded configurations
   to IKEv2. `#4873 <https://redmine.pfsense.org/issues/4873>`__
-  Specify rightid for mobile IPsec non-PSK configurations. Add peer ID
   option "any" for excluding peer identifier checks for mobile IPsec
   circumstances where peer ID matching is impossible or undesirable.

OpenVPN
-------

-  Fixed handling of OpenVPN automatic stop/start when bound to gateway
   groups using CARP VIPs.
   `#4854 <https://redmine.pfsense.org/issues/4854>`__

DHCP
----

-  Fixed issues with IPv6 Prefix Delegation caused by an invalid
   prefix/subnet check added to the ISC DHCP daemon. Reported upstream
   and patched the checks out in FreeBSD ports.
   `#4829 <https://redmine.pfsense.org/issues/4829>`__

DNS Resolver
------------

-  Changed Unbound to use interface-automatic where interface list is
   empty so it replies correctly in a default HA configuration.
   `#4807 <https://redmine.pfsense.org/issues/4807>`__
-  Fixed selection of a CARP VIP for outgoing interface.
   `#4852 <https://redmine.pfsense.org/issues/4852>`__
-  Fixed some inconsistencies in text across the GUI in places that
   specified DNS Forwarder vs. Resolver.
   `#4551 <https://redmine.pfsense.org/issues/4551>`__

Load Balancer
-------------

-  Improved handling of port ranges in relayd.
   `#4810 <https://redmine.pfsense.org/issues/4810>`__
-  Fixed references to Load Balancer Virtual Server *redirect_mode*.

Traffic Shaping
---------------

-  Fixed adding of VoIP rules from traffic shaper wizard where IP/alias
   was not specified.
   `#4838 <https://redmine.pfsense.org/issues/4838>`__
-  Fixed default CoDel values.
-  Corrected inverted target/interval values for CoDel.

Rules/NAT/Aliases
-----------------

-  Fixed a foreach() error when saving an empty alias.
-  Fixed input validation on Alias import page.
-  Fixed inconsistencies in descriptions in Alias editing for URL Table
   aliases.
-  Added labels to more default firewall rules.
-  Avoid an error loading the rules with a numeric hostname in an alias.

Misc
----

-  Removed unnecessary deletion of rc.conf; Added an empty rc.conf with
   a note.
-  Removed a check for a QinQ interface existing when deleting. The
   check unnecessarily made QinQ un-deletable where the parent interface
   no longer existed.
-  Fixed GratisDNS support.
-  Fixed glob for serial devices to match more accurately.
-  Fixed a foreach() warning when editing PPP entries.
-  Fixed GRE and GIF interface input validation so required fields and
   descriptions match.
-  Changed the behavior of Cancel buttons to be consistent (return to
   referring page).
-  Fixed display of advanced DHCP settings when present.
-  Removed old, unused *NetUtils.js*.
-  Retain */usr/bin/fsync* from FreeBSD in images.
-  Added "netstat -ni" to /status.php output.
-  Fixed a typo in upgrade code for Captive Portal.
-  Fixed limiter upgrade code to allocate pipe numbers even if no rules
   are present.
-  Fixed upgrade code to remove old CA/Cert config entries that were
   moved/relocated.

