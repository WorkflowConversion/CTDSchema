# Generating a CTD for wget
We assume you've already got installed a version of [wget]. 

Wget is a simple tool to obtain files given an URL.

The first step is to decide which parameters will be considered. Let us start simple with one simple parameter, `o`, which determines the output location of a downloaded file. In other words, we will be able to wrap the following functionality: 

```sh
$ wget --output-document http://www.sample.com/files/one.txt
```

Let's start with our CTD. We will map the following parameters:

* `--output-document`: this is to specify where do we want the downloaded data to land.
* `--no-verbose`: a flag to control how much information is produced.
* `--no-check-certificate`: a flag to skip SSL certificate checks.
* The input URL. We will talk about this one a bit later.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<tool version="1.17.1" name="wget" category="Data" ctdVersion="1.7">
    <description>Wget - The non-interactive network downloader.</description>
    <executableName>wget</executableName>
    
    <PARAMETERS version="1.7">
        <NODE name="wget" description="Parameters for wget">
            <ITEM name="outputDocument" type="output-file" description="Loction of the output file" required="true" value=""/>
            <ITEM name="noCertificate" type="bool" description="Skip checking of certificate" required="false" value="false"/>
            <ITEM name="noVerbose" type="bool" description="Print important information" required="false" value="true"/>
            
            <!-- url is a positional parameter, so it should be at the very end!!! -->
            <ITEM name="url" type="string" description="URL to download" required="true" value=""/>
        </NODE>
    </PARAMETERS>
    
	<cli>
		<clielement optionIdentifier="--output-document">
			<mapping referenceName="outputDocument" />
		</clielement>
		
		<clielement optionIdentifier="--no-check-certificate">
			<mapping referenceName="noCertificate" />
		</clielement>
		
		<clielement optionIdentifier="--no-verbose">
			<mapping referenceName="noVerbose" />
		</clielement>
		
		<!-- leave this one blank on purpose -->
		<clielement optionIdentifier="">
			<mapping referenceName="url"/>
		</clielement>
	</cli> 	   
</tool>
```

If you see the `<cli>` section, you'll see the following mappings:

* The `outputDocument` parameter is mapped to the `--output-document` command line option.
* `noVerbose` is mapped to `--no-verbose`.
* `noCertificate` is mapped to `--no-check-certificate`.
* `url` is mapped to an empty command line option, this is tue do the fact that it is a positional parameter, therefore it was also included at the end of the `<NODE>` section.

This simple CTD file can be used in several platforms, such as [Galaxy], [KNIME]. Take a look at [GenericKnimeNodes] and [CTD2Galaxy] to see how a CTD can be imported into these systems.


# Tools that are fully CTD compatible.
[BALL], [OpenMS] and [SeqAn] are bioinformatics libraries that are fully compatible with CTDs. This means that not only their tools generate valid CTDs, they can also _parse_ CTDs.


[wget]: https://www.gnu.org/software/wget/
[Galaxy]: https://wiki.galaxyproject.org/
[KNIME]: https://www.knime.org/
[GenericKNIMENodes]: https://github.com/genericworkflownodes/GenericKnimeNodes
[CTD2Galaxy]: https://github.com/WorkflowConversion/CTD2Galaxy
[BALL]: https://github.com/BALL-Project/ball
[OpenMS]: http://www.openms.de/
[SeqAn]: https://github.com/seqan/seqan