## Internal Connectors

These bad boys are the internal workhorses that do all the dirty work of enriching stuff on OpenCTI. They're like a bunch of sweaty laborers digging through piles of data to find those precious nuggets of information. These connectors are tightly integrated with OpenCTI, giving them unrestricted access to all the nooks and crannies of its inner workings. They're the undercover agents of data enrichment, extracting valuable intel from every corner, and leaving no stone unturned. 

These connectors are faster than a cheetah on steroids, pumping up OpenCTI with more power and badassery than ever before. They'll blow your mind with their efficiency and effectiveness. So buckle up, you sickos, because when it comes to enriching your data, these internal connectors are the real deal!


# IVRE Custom Configurations

## ivre_custom_config.conf

The `ivre_custom_config.conf` file is where the magic happens for the IVRE connector on OpenCTI. It's like the control center for all the badass enrichment work. Take a look at what's inside:

```conf
DB = "mongodb://ivredb/"

IPDATA_URLS = {
    "GeoLite2-City.tar.gz": None,
    "GeoLite2-City-CSV.zip": None,
    "GeoLite2-Country.tar.gz": None,
    "GeoLite2-Country-CSV.zip": None,
    "GeoLite2-ASN.tar.gz": None,
    "GeoLite2-ASN-CSV.zip": None,
    "GeoLite2-dumps.tar.gz": None,
    "iso3166.csv": "https://dev.maxmind.com/csv-files/codes/iso3166.csv",
    "BGP.raw": "https://thyme.apnic.net/current/data-raw-table"
}

MAXMIND_LICENSE_KEY = open('/run/secrets/maxmind_license_key').read().strip()

```

## ivre_init.conf

The `ivre_init.conf` file is your go-to initiation script for IVRE. It's like the grand ceremony that kicks off all the epic scanning and viewing. Check out the code snippet:

```conf 
#!/bin/bash

yes | ivre ipinfo --init
yes | ivre scancli --init
yes | ivre view --init
yes | ivre flowcli --init
yes | ivre runscansagentdb --init
ivre ipdata --download --import-all

```


## maxmind_license_key.conf

Last but not least, we have the `maxmind_license_key.conf` file. It holds the key to unlock the power of MaxMind's magical data. Don't forget to fill in your MaxMind license key:

```
### ENTER MAXMIND LICENSE KEY HERE
```


With these custom configurations, you're ready to unleash the true potential of IVRE on OpenCTI. Use them wisely, my friend, and may your data enrichment endeavors be filled with excitement and success!