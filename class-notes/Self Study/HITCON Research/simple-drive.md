PRNG Pseudo-Random Number Generator

1. What is this server? A webpage? A machine?
	- It is both. The Machine: It is a remote computer (a Linux server) hosted in the cloud specifically for this CTF competition.The Webpage: That machine runs a web application (a website). Anyone with the URL can open it in a web browser like Chrome or Safari.
2. What does it do?
	- It is a simple Cloud Storage Drive (similar to a very bare-bones Google Drive or Dropbox). When you register an account and log in, the server gives you a private folder on its hard drive.Inside this folder, you can create sub folders and organize your text files.

3. How does the user interact with it?

	You don't only upload ZIP files. A normal user interacts with the website using a standard web browser or by sending HTTP web requests via scripts. The application gives you a few features (or "routes"):
		• /register and /login (To create an account and log in)
		• /mkdir (To create a new folder)
		• /backup (To download your files)
		• /restore (To upload a backup)


4. How do the ZIP files get involved? (Who sends what?)
	The server handles ZIP files through the Backup and Restore features. It goes in a specific loop:
	- The Server sends the ZIP first (Backup): A user clicks the "Backup" button on the webpage (/backup). The server gathers all the user's uploaded files, bundles them into a .zip archive, signs it, and sends it to the user to download as a safety copy.
	- The User sends a ZIP back (Restore): If a user accidentally deletes their files later, they can go to the "Restore" page (/restore) and upload that exact same signed .zip backup file. The server checks the signature to make sure the user didn't modify it, opens it up, and restores the files.

5. How does the server even sign the ZIP file?

The server uses a custom archive format. When you click /backup, the server builds the file like a sandwich:

1. Section	What it contains	Who makes it?
2. Header	Meta-data about the files + a CRC (tamper seal number).	
	- Created by the server.
3. Payload	Your actual files compressed together.	
	- Created by the server.
4. Footer	The Digital Signature (\(r, s\)
	- Calculated by the server using its Master Key and a random number \(k\)


both of these needs the PRNG + MT19937 algorithm 
Session ID -> Hacker logs in and out to get a pattern and predict

Because we have the algorithm mapped out,
Nonce -> Created by MT19937 + the K number

we can correctly guess the K number.


