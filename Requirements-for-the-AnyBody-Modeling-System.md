Computer Requirements for the AnyBody Modeling System v6.0.2

+ [Operating system](#operating-system)
+ [Memory](#memory)
+ [Processor](#processor)
+ [Graphics](#graphics)
+ [Hard drive](#hard-drive)
+ [Web Browser](#web-browser)
+ [SolidWorks](#solidworks)
+ [Python](#python)


## Operating system:
AnyBody runs only on standard Windows PC computers with Windows Vista, Windows 7 or Windows 8 operating systems.
AnyBody may work on earlier Windows operating systems, but this is not supported.
The 64 bit version of AnyBody requires a 64 bit Windows to run.

## Memory:
AnyBody will in principle run on very little memory, but if you have less than 128 MB available to AnyBody, you will only be able to run very small models. Larger models typically require 256 MB or more, and a full-body model does not run well on less than 512 MB.

• Working with full-body models, a minimum of 2GB ram is required, and 4GB recommended.

• Notice that from version 5.0, the 32 bit version of AnyBody is built with Large Address Awareness meaning that it can use more than the standard 2GB of memory for 32 bit programs. On 64 bit Windows systems, the 32 bit version AnyBody can use up to 4GB and on 32 bit Windows systems proper modification of system options can bring it to use up to 3GB of memory. The 64 bit version of AnyBody can utilize the complete amount of memory available on a 64 bit Windows.

• Running large models with large data sets, e.g. from MOCAP with many samples may required gigabytes of memory;

## Processor:
The system should work on any processor capable of running the recommended operating systems, but the faster, the better. Large models are computationally demanding.

## Graphics:
A graphics adapter with OpenGL 1.1 support is a minimum requirement, but it is strongly recommended to have a discrete graphics adapter supporting OpenGL 2.0. In particular, graphics performance may be slow on shared-memory graphics adapters. At least 128Mb of dedicated memory on the graphics adapter is recommended for running large models such as the full-body models from the model repository. On Windows systems with multiple graphics adapters (e.g. an on-CPU graphics adapter and a discrete graphics adapter), it is recommended to specify in the system’s graphics setup that the
discrete graphics adapter should be used for the AnyBody Modeling System.

•  Exporting very high-resolution images for posters and the like requires substantially more dedicated graphics memory, at least 512MB is recommended. Such exports also require additional computer memory; 1 GB of extra computer memory can be required during the export.


## Hard drive:
1.0GB of free disk space is recommended for installation and model work. It is primarily needed when working with full-body models.

## Web Browser:
The AnyBodyTM Modeling System requires that Microsoft Internet Explorer 9.0 or later is installed on the computer; newest version is recommended. Older versions may result in reduced functionality of the AnyBodyTM GUI, but the core simulation functions are not affected.


## SolidWorks:
The SolidWorks2AnyBody translator add-in requires SolidWorks to be installed to work. SolidWorks2AnyBody is built for SolidWorks 2012. It may function on earlier or newer versions as well due to the generality of the SolidWorks API, but this is not supported.

## Python:
The AnyBody Modeling System implements an interface for using Python functionality in models. To use this optional feature, you will need to have Python 2.6 or 2.7 installed. The 32 bit version of AnyBodyTM can interface with a 32 bit Python
installation, and the 64 bit version of AnyBodyTM can use a 64 bit Python installation.