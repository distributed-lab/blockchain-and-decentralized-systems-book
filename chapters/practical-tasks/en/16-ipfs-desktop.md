# 16. IPFS Desktop Client
1. Click on the link and download the client software for your operating system.
https://github.com/ipfs-shipyard/ipfs-desktop
2. Launch IPFS

![Img](/resources/img/practical-volume/16/22-launch.png)

3. Downloading target files from the network occurs in the following way. Firstly, to download something from
the network, you need to have a multihash file that you want to download. There are various files available
on the network that can be downloaded using IPFS, but for example, you can first obtain data with the
following: **QmR7GSQM93Cx5eAg6a6yRzNde1FQv7uL6X1o4k7zrJa3LX**.

![Img](/resources/img/practical-volume/16/23-get-data-by-id.png)

4. You can see the size and number of data blocks that the target data is divided into. You can also
look at the visualized tree of the division of target data. In this case, the data is represented by a single data block.

![Img](/resources/img/practical-volume/16/24_file_tree.png)

5. To view and download the file, follow these steps. Paste this multihash 
**QmR7GSQM93Cx5eAg6a6yRzNde1FQv7uL6X1o4k7zrJa3LX**. Click _"Browse"_.

![Img](/resources/img/practical-volume/16/25-get-data-by-id2.png)

6. Now we can view the content of the file _ipfs.draft3.pdf_. To do this, click on it.

![Img](/resources/img/practical-volume/16/26-file.png)

![Img](/resources/img/practical-volume/16/27-display.png)

7. To add a file to the IPFS network, go to Files. Click on _"Import"_. Select the necessary file/folder and upload it

![Img](/resources/img/practical-volume/16/28-import.png)

8. You see a list of all the files that you have downloaded.

![Img](/resources/img/practical-volume/16/29-uploaded.png)

9. Find the CID Info of the uploaded files.
10. Also try uploading a large file. See how it was added to the network, and what type of file it was (blob, list, tree, commit). How the data is distributed.
11. Study the _"Settings"_ section and find your PeerID.
12. Find the files that other students have uploaded.
13. Go to the _"Peers"_ section. Study what information you can get in this section.
14. You can also configure the IPFS network using the command line. To do this, 
go to  https://docs.ipfs.io/install/command-line/ and follow the instructions for your OS.
15. Then go to the next link and follow the instructions to initialize the repository and bring your
node online: https://docs.ipfs.io/how-to/command-line-quick-start/#prerequisites .