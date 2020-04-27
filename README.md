# AirPdfPrinter
Virtual PDF AirPrint printer

## HOWTO

* Build

  ```bash
  # Assume you're in this project's root directory, where the Dockerfile is located
  docker build -t air-pdf-printer .

  # Build with argument, set your own admin password instead of the default one
  docker build --build-arg ADMIN_PASSWORD=<YourPassword> -t air-pdf-printer .
  ```

  Default admin password is [here](https://github.com/thyrlian/AirPdfPrinter/blob/master/Dockerfile#L23)

* Run

  ```bash
  # Run a container with interactive shell (you'll have to start CUPS print server on your own)
  docker run -it -p 631:631 -p 49631:49631 -v $(pwd)/pdf:/root/PDF -v $(pwd)/cups-pdf:/var/spool/cups-pdf --name air-pdf-printer air-pdf-printer /bin/bash

  # Run a container in the background
  docker run -d -p 631:631 -p 49631:49631 -v $(pwd)/pdf:/root/PDF -v $(pwd)/cups-pdf:/var/spool/cups-pdf --name air-pdf-printer air-pdf-printer
  ```

* Troubleshoot

  Logs directory: `/var/log/cups/`

* Commands

  ```bash
  # Start CUPS service
  service cups start

  # Shows the server hostname and port.
  lpstat -H

  # Shows whether the CUPS server is running.
  lpstat -r

  # Shows all status information.
  lpstat -t

  # Shows all available destinations on the local network.
  lpstat -e

  # Shows the current default destination.
  lpstat -d
  ```

* Manage

  Web Interface: http://[*IpAddressOfYourContainer*]:631/

* Add Printer

  * **macOS**: `System Preferences` -> `Printers & Scanners` -> `Add (+)` -> `IP`

    * **Address**: [*IpAddressOfYourContainer*]
    * **Protocol**: `Internet Printing Protocol - IPP`
    * **Queue**: `printers/PDF` (find the info here: http://[*IpAddressOfYourContainer*]:631/printers/)
    * **Name**: [*YourCall*]
    * **Use**: `Generic PostScript Printer`
