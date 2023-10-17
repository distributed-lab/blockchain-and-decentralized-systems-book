# Working with the decentralized data storage system IPFS

-> In the first chapter of the second part of the textbook 'Blockchain and Decentralized Systems', we
introduced readers to the architecture and features of the IPFS protocol, its main principles of operation, and its use [...].

This section will be dedicated to how to practically utilize the advantages of this technology. IPFS
(InterPlanetary File System) is a network communication protocol that implements a content-addressed,
peer-to-peer hypermedia system for decentralized data storage. IPFS is an open-source project developed 
by Protocol Labs with the participation of the open-source community. It was initially developed by
Juan Benet. IPFS is a peer-to-peer distributed file system that unifies all computing devices into a
single file system. IPFS combines a distributed hash table, decentralized exchange blocks, and self-certifying
namespaces. IPFS does not have any points of failure, and nodes are not required to trust each other.
The difference from other decentralized networks is that the block is the self-contained unit that is
transmitted on the network. The block can contain either part of a file or links to other blocks. A directed
acyclic graph is produced from the blocks, from which a file or directory is subsequently assembled.
Search by file or directory name in IPFS is absent, as it is in the BitTorrent network. This system 
allows for a more flexible approach to data storage and transmission in the network.

## Content-addressable hyperlinks
The IPFS protocol uses content addressing, which allows addressing this content unambiguously in the
network. Such addresses include the hash value of the content of another type, where the hashing 
algorithm and the  hash value length are specified. The protocol for working with such hash values is
called Multihash. Multihash is a structure consisting of three parts: the ID of the hash function, the
length of the hash value in bytes, and the hash value of the target content.

![Img](/resources/img/practical-volume/15/1-hyperlink.png)

## IPNS Name Service
IPFS has a service called IPNS, which is the name of a global namespace based on open keys, compatible
with other namespace names, and can integrate DNS, .onion, .bit, etc., into IPNS.

To become familiar with the IPFS protocol in detail, it is necessary to have any distribution of the
Linux operating system installed (both virtually and actually) and the appropriate software - a configuration
script for automatic startup.

Under the configuration script of automatic startup, there is a utility called “dlab”, which was 
specially designed to perform this practical block and is available for download at the
[link](https://github.com/OlKurbatov/dlab_tool) (`git pull`).

The script performs the following functions:
* automatic configuration of private networks for various blockchain-based protocols (with several
deployment options);

* automatic launch and connection of additional nodes to the private network at the student's request;

* conducting load tests for maximum approximation to the conditions of a real decentralized educational
system.

Essentially, this script allows you to start the manual network configuration stage and immediately begin familiarizing
yourself with the protocol.

After you have a working Linux distribution with installed software (dlab), you should follow these steps to configure a 
private Bitcoin network:
1. Open the terminal.

2. Invoke the configuration script of the "dlab" command from https://github.com/OlKurbatov/dlab_tool.
 **NOTE: TO PROPERLY SHUT DOWN NODES IN THE CONSOLE WINDOW OF THE CONFIGURATION SCRIPT, USE THE KEYBOARD COMBINATION ctrl+C**.

3. Choose the type of network (in this case, IPFS).

![Img](/resources/img/practical-volume/15/2-config.png)

4. Choose the type of network deployment (start IPFS console with console).

![Img](/resources/img/practical-volume/15/3-depltype.png)

5. You will see a console with two tabs: one will display daemon logs of the ipfs process, and in the other, you will
directly interact with IPFS and perform operations such as uploading (add), downloading (get), pinning, 
as well as managing local storage and current settings.

![Img](/resources/img/practical-volume/15/4-consolas.png)

When the ipfs daemon is first launched, the process creates _.ipfs_ a folder in the user's directory
on behalf of which the process was launched. Blocks that have been synchronized with the network, DHT,
and node settings will be stored in this directory
![Img](/resources/img/practical-volume/15/5-daemon.png)

When launching IPFS, it will display information about the addresses on which it will wait for incoming 
connections.

6. Go to the cli_console tab and enter the command `id` - it will display information about the website.

![Img](/resources/img/practical-volume/15/6-id.png)

Where:

`ID` - unique identifier of the node;

`PublicKey` - public key (RSA);

`Addresses` - addresses at which the node is waiting for incoming connections;

`AgentVersion` - agent version (go-ipfs - a command-line interface implementation written in Go);

`ProtocolVersion` - protocol version.


7. Use the `repo stat` command, which will display the current state of the node repository.

![Img](/resources/img/practical-volume/15/7-repostat.png)

Where:

`NumObjects` - the number of blocks stored;

`RepoSize` - the total amount of data stored in the repository;

`StorageMax` - the maximum amount of data that can be stored;

`RepoPath` - the path to the repository;

`Version` - the version of the repository.


8. As you know, IPFS does not permanently store data blocks of other nodes (unless you pin a file). After a certain
period of time, a garbage collection procedure is automatically performed. However, we can manually
initiate this process using the "repo gc" command. The output of the command will show the multi-hash
of the file being deleted (or parts of the file).

![Img](/resources/img/practical-volume/15/8-remove.png)

After executing this command, review the repository information again (`repo stat`) and make sure that the 
`NumObjects`value has become close to zero.

9. Now let's upload something to the network. To do this, select a file (for example, an image) and execute the command
`add <path to image>`.

![Img](/resources/img/practical-volume/15/9-upload.png)

In response, the team has provided you with the multi-hash of the recently uploaded file and its size.
Along with adding the file to the network, the node stores it in its own repository (so that garbage
collection does not delete it). After the file is uploaded to the network, you can share its multi-hash
with your friend so that they can download it and become a host for the file.

10. The Ipfs daemon consists not only of a set of console tools, but also a web panel that provides less functionality,
but allows visualizing data obtained from the network. To access it, you need to open any web browser
and enter the address `localhost:5001/webui` in the address bar.

![Img](/resources/img/practical-volume/15/10-browser.png)

Here you can see the download and upload speed of files in the network (blocks of other nodes constantly pass through the node),
view information about a file in the network (using its multihash), see the peers you are connected 
to, as well as upload files to the network and manipulate them.

11. Let's check the speed of uploading target files to the network using the `upload test`. Go to the
console window from which you uploaded the ipfs node and select `upload test`.

![Img](/resources/img/practical-volume/15/11-upload-test.png)

12. You will see a console window in front of you, in which you have to select the parameters for launching the test.
* Choose a folder for importing files - select a folder on your local machine, from which all files will be uploaded 
to the IPFS network. 
* Generate files to import - generate files for uploading in a specified quantity and range of size.

We prefer to generate files because, in this case, the sample will more clearly show the dependence of the loading time 
of target files on their size. Choose the first option (enter 2 and press Enter).

Select the number of files to create:

![Img](/resources/img/practical-volume/15/12-files.png)

Select the maximum file size for creating:

![Img](/resources/img/practical-volume/15/13-maxsize.png)

Your values for starting the test may differ from the values in the screenshots. After generating the files, their uploading
to the network will begin.

![Img](/resources/img/practical-volume/15/14-files-generation.png)

Please wait for the download process to complete. A window with a graph will open in front of you, where each point represents a file. 

![Img](/resources/img/practical-volume/15/15-chart.png)

The dependence of the target file's download time on its size is clearly traced here. In fact, the
concept of "downloading" to the network in the context of IPFS is abstract, as the files being downloaded
essentially continue to be stored on your computer. However, they obtains a multihash and, depending
on their size, can be divided into several related blocks

13.  Downloading target files from the network happens differently. Firstly, in order to download anything from the 
network, you need to have the multi-hash of the file you want to download. There are various files
that can be downloaded using IPFS on the network, but let's work with this multi-hash:
_QmR7GSQM93Cx5eAg6a6yRzNde1FQv7uL6X1o4k7zrJa3LX_. To start, let's insert it into the IPFS network 
explorer: go to the web interface (see Step 10) and paste the multihash into the search bar of the explorer.

![Img](/resources/img/practical-volume/15/16-search.png)

Press Enter and view the information obtained from Explorer. You can see the size and number of data blocks that the 
target data is divided into. In this case, it is a directory that contains files.

![Img](/resources/img/practical-volume/15/17-datablocks.png)

You can also look at the visualized tree of the division of target data. In this case, the data is presented as a single
block of data.

![Img](/resources/img/practical-volume/15/18-oneblock.png)

14. Now let's download the directory with the multihash and find out about its contents. To do this, go to the IPFS network
node console in the cli_console tab and enter the command: `get <hash>`, where <hash> is the multihash
of the target data on the IPFS network.

![Img](/resources/img/practical-volume/15/19-get-file.png)

The directory has been loaded, now you can view its contents using the command `ls <hash>`, where
<hash> is the multihash of the target data. 

![Img](/resources/img/practical-volume/15/20-lshash.png)

It can be seen that there is a file called ipfs.draft3.pdf in the directory. Let's move it to the local machine using 
the command `cat HASH/file.extension > destination folder/file.extension`, where:
* Cat - command to display the contents of target data;

* HASH - multi-hash directory (if the multi-hash represented a file, we would display its contents);

* HASH/file.extension - a file that is displayed (since we have HASH as a directory, we refer to the file that is located under it);

* '> - output operator;

* `> destination folder/file.extension - path and file name to which the contents of the target data
will be written.

In our case, the team will have the following format: `cat QmR7GSQM93Cx5eAg6a6yRzNde1FQv7uL6X1o4k7zrJa3LX/ipfs.draft3.pdf > /your/path/whitepaper.pdf`  
(replace /your/path with your path on the local machine).

15. Now go to the path indicated in /your/path and open the file whitepaper.pdf. You should see the whitepaper IPFS open in front of you.

16. If you have access to another machine with a running IPFS node, you can exchange any file between these two nodes. 
To do this:

* Select the target file (preferably an image or small document).
* Use the command `add /path/to/file.extension` to make the target file available to other network users.
* Copy the resulting multihash and transmit it to another node on the network by any means.
* On another node, use the command `get <file_hash>` to download the target file and, by analogy 
(point 14), save the file to the local machine. There is a parameter `--output=/some/path` that can 
be used to specify the IPFS node to save the target file directly to the specified path on the local
machine. Usage: `get --output=/home/user/Documents <HASH>`.
* Open the target file from the second node. Now the second node is the host for the target file.
This can be verified using the command `dht findprovs` (finding nodes that provide this file will take
some time).

![Img](/resources/img/practical-volume/15/21-findproves.png)