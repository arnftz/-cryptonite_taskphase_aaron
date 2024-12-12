# Forensics

## tunn3l v1s10n (Medium)
### Flag
* flag: picoCTF{qu1t3_a_v13w_2020}
### Thought Process
* downloaded and opened the file, absolute gibberish nothing was legible, did a hex dump, figured out it was bitmap
* i am familiar with the fact that files have a header, even if its not legible, the header will be visible
* after googling i realised that BM stands for bitmap image files, now the question was how do i fix it
* i saved it as a .bmp file and tried opening, it didnt work
* which made me draw the conclusion that the image is corrupted, possibly a header issue (also i heard whispers about it)
* to learn more about bitmaps i opened [this](https://www.ece.ualberta.ca/~elliott/ee552/studentAppNotes/2003_w/misc/bmp_file_format/bmp_file_format.htm) link.
* i also used hexedit on my wsl to read and edit the header of the bitmap
* the first two letters, seemed to be correct 'BM'
* the next bytes were supposed to be the size of the file, but here it was random gibberish
* so to confirm i opened a normal bitmap file off the internet
* also another funny thing i found while tryna decode the header section, in two places it is written BA D0 00 00 which is basically BAD spelled out
* so i replaced them with the template bitmap file header, and i got the image to display but it showed picoCTF{notaflag}, but there was another pink image attached to the file but not visible in range which made me think maybe i got the size hexadecimals wrong, what if i make it bigger
* modifing the width for some reason just distorted the image so i tried messing with the height, initially i didnt get the flag but the image was no longer distorted and the size was responsive to the changes
* first i changed it to 1000px it failed, 500px it increased height but no flag, so i tried 750 px, then 825px and bingo
* found the flag
### Steps Taken
* Open Flag in hexedit
* fix the dataoffset and size of infoheader
* set the height of the bmp file to 825px and find the flag

## m00nwalk (Medium)
### Flag
* flag: picoCTF{beep_boop_im_in_space}
### Thought Process
* listening to the audiofile, the noise made not much sense
* looking into the hint of how images from moon were sent to earth, i found that they converted the image into radio signals and sent them to earth, now this info wasnt much so i dived deeper
* how were the radio signals sent, what method was used to encode and decode, then i came across the slow scan tv method which was especially used during guess what, THE FIRST MOONWALK, istg the punnery in ctf.
* i didnt bother diving deeper into the theory, basically they transmit the images horizontal line by line using radio wave signals embedded in audio or some shit, which is what WE HAVE :) yay
* so then it was a simple google of how to convert wav file into image using sstv method and i found qsstv for ubuntu
* installed it, found [this](https://ourcodeworld.com/articles/read/956/how-to-convert-decode-a-slow-scan-television-transmissions-sstv-audio-file-to-images-using-qsstv-in-ubuntu-18-04) tutorial on how to use it
* after following a tedious amount of age old steps from the last century and some modifications from my end, i was able to decode the image and find the flag
### Steps Taken
* configure sstv through its numerous steps
* play the audio file into the sstv program
* set the RX to Scottie 1
* retrieve flag from the image

## m00nwalk (Medium)
### Flag
* flag: picoCTF{h1dd3n_1n_pLa1n_51GHT_18375919} 
### Thought Process
* with my limited experience in wireshark in the last decade i recognize the capture file
* i see that a few files were transfered using the TFTP, so i filter out those lines only
* then i google how to download these files, you go to files and export all files transfered through TFTP
* opened the instructions file, it is encoded in some format, probably some form of a shift cipher
* so i open a shift cipher website, and change the shift from 1 to 2, 3, 4... until i reached 13 and it hit me ROT 13 (im such an idiot)
* TFTPDOESNTENCRYPTOURTRAFFICSOWEMUSTDISGUISEOURFLAGTRANSFER.FIGUREOUTAWAYTOHIDETHEFLAGANDIWILLCHECKBACKFORTHEPLAN
* the above text i think is indicating to the plan file
* the plan file is also encrypted, possibly again a shift cipher, i started again with ROT13 and i was right
* IUSEDTHEPROGRAMANDHIDITWITH-DUEDILIGENCE.CHECKOUTTHEPHOTOS
* so i open the photos
* some landscape photos, i presume obviously steganography
* checking out the program.deb, it kept showing errors that a certain file steghide wasnt configured properly
* did some googling, realised i could extract the flag using the module steghide
* when i tried the command it asked for a pass phrase
* i was stumped for a second, but i have experience with these stupid riddles by now
* i went back to the clues, and realised instantly this time
* I USED THE PROGRAM AND HIT IT WITH - "DUEDILLIGENCE"
* ffs, anyways i ran the command on all three bitmap images and it worked on the third image
* got the flag from a text file
### Steps Taken
* open the wireshark file
* filter the tftp.type files
* export all files transfered using tftp
* decode the instructions and the plan file using rot13
* use steghide on the third image using DUEDILIGENCE as passphrase
* voila

