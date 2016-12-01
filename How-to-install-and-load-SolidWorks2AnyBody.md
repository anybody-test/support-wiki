## Installation and License

SolidWorks2AnyBody is installed on your hard disk from the regular AnyBody™ installer.

In order to make sure that the add-in is installed, you must choose to either do a complete AnyBody™ installation,
or you must specifically select to install the SolidWorks2AnyBody™ add-in from the Tools section when doing a custom installation.

![image](https://cloud.githubusercontent.com/assets/22542671/20789662/8f9b3432-b7b5-11e6-87a6-1fccf9b9eba3.png)

SolidWorks2AnyBody requires a separate license, which is typically located in the same file as your AnyBody™ license.

Importing the license file into AnyBody™ through the license manager will place the file in the proper position for SolidWorks2AnyBody™ to find it.

# Getting Started

## Loading the plug-in

SolidWorks2AnyBody is delivered in the form of a DLL (Dynamic Link Library), which can be loaded as an add-in from SolidWorks by doing the following steps.

1. Start SolidWorks with administrator privileges (by using the "run as administrator" option in Windows).
2. Choose the menu ‘File’->’Open’.
3. In the file dialog, select ‘Add-Ins (*.dll)’ file extension type and choose the SolidWorks2AnyBody™ DLL file.

![image](https://cloud.githubusercontent.com/assets/22542671/20789704/bc8c0c64-b7b5-11e6-862d-01b37ce5fe13.png)

The SolidWorks2AnyBody™ DLL can be found in the AnyBody™ installation, typically in the folder ‘C:\Program Files (x86)\AnyBody Technology\AnyBody.X.X\Tools\SolidWorks2AnyBody’, where X.X indicates the AnyBody™ version number.

After you have loaded the SolidWorks2AnyBody™ module successfully, you can see that SolidWorks2AnyBody™ module appears in SolidWorks’ menu bar.

![image](https://cloud.githubusercontent.com/assets/22542671/20789719/d01313fe-b7b5-11e6-8440-4d9ecd611c04.png)

You are now ready to use SolidWorks as a powerful model authoring tool for the AnyBody Modeling System. It is only necessary to use "run as administrator" when loading the add-in, afterwards SolidWorks can be started normally.

## Translating a CAD model

We imagine that an assembly of an exercise machine has been created in SolidWorks as shown in the figure below. An assembly in SolidWorks is a set of parts connected by mates, which are really kinematic constraints that, for instance, keep a shaft in a bearing or make two faces parallel. This forms a mechanism. The translation process generates an AnyBody™ segment from each of the parts and connects the segments with joints corresponding to the mates. To carry out the translation, please perform the following steps.

![image](https://cloud.githubusercontent.com/assets/22542671/20789745/e3940fc8-b7b5-11e6-8622-a993daf9af62.png)

1) In the SolidWorks2AnyBody™ pull-down menu, choose the ‘Export to AnyScript’ operation.

![image](https://cloud.githubusercontent.com/assets/22542671/20789755/f174419e-b7b5-11e6-814b-881a8996510c.png)

2) A dialog with export options appears.

![image](https://cloud.githubusercontent.com/assets/22542671/20789768/fe60e808-b7b5-11e6-9bb5-153d61d9e4d1.png)

3) Next, type the name of the AnyScript™ (.any) file to be created. Then click the ‘Save’ button.

![image](https://cloud.githubusercontent.com/assets/22542671/20789788/0c87febc-b7b6-11e6-9845-5538abaae427.png)

The model is now saved as AnyScript™ files and other related files, which are ready for usage in the AnyBody Modeling System. The selected file name is root file needed for inclusion into AnyBody™ models.

All other related files have that same first name but with an extension before the actual file extension. All files are in the same folder as the root folder.