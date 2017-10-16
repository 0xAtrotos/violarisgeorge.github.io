## Simple blockchain logic in Go

I've created a small Go program which demonstrates how blocks are added to a blockchain. It can help someone learn how the data structure is created and how each block is linked to the next.

If you've got a nice IDE it might go ahead and create the imports for you. Here they are to import them yourself. We'll need the crypto/sha256 package to feed it with strings and it will output a SHA256 hash for us. Keep in mind that when it comes to hashing, it is best to choose the newer cryptographic hash functions and avoid the older ones. This is because md4, md5 and SHA1 have all been broken. It is now relatively easy to manually create collisions for the three mentioned here. 

<img src="https://violarisgeorge.files.wordpress.com/2017/10/wp1.png" alt="wp1" width="245" height="202" class="alignnone size-full wp-image-88238" />

SHA256 generates a nice 32-byte (256-bit) string, which is considered fairly unique. It's very highly unlikely that two different strings or documents or files will digest to the same hashed string. For instance, with the now broken SHA1, it is possible for Alice to create a file that matches the hash of another file. In order to achieve this, she takes the hash of a good file and by brute-force adds or removes bits to a malicious file until she finds a configuration whose hash collides with the hash of the good file. If Bob trusts SHA1, he can be deceived into thinking that he has the right file while he actually has Alice's malware file.

A blockchain, is a data structure made up of linked blocks. What are blocks and how are they linked? Well, a block in bitcoin is a container of financial transactions. However, we can make our blockchain to contain any kind of information we wish, such as text data, images, numbers, or even other data structures such as lists, trees, and so on. At this point you may be wondering where hashing comes into the question and why we've been introduced with SHA256 in the beginning of this post. Blockchains are very unique data structures, in that every single block on the chain is only linked to one, and not more, previous blocks. The reason for this boils down to what we need our data structure to be used for. Blockchains are useful for keeping track of the timing that a piece of data has been added to memory, to a database, or announced to a network. Thus, the most obvious use case of a blockchain is for financial applications such as cryptocurrencies, payment processing, smart contracts, ownership tokens, and so on. SHA256, or any other hashing function that hasn't been broken yet, is useful in creating a string to act as a pointer to the previous block. Since hash functions are one-way functions that produce different strings when we change even 1 bit, it is a way of being sure that a certain hash used as a pointer to a block will not be recreated in the system. Therefore we don't have the risk of running into a block being pointed at by more than one future block.

For our purposes, to create our block I am using a struct that creates a Block with an index, a timestamp, some data, and the hash of the previous block (i.e. you take the whole block, pass it through SHA256, and the hash is handed down in the next Block).

<img src="https://violarisgeorge.files.wordpress.com/2017/10/wp2.png" alt="wp2" width="791" height="389" class="alignnone size-full wp-image-88239" />

The first block is called the "Genesis" block as per Bitcoin lore. Since there's no previous block before the Genesis block, it is trivial to create our first hash by simply doing a SHA256 on a string. This string can be anything you want, it can be some random text, a sentence from your favorite poem, the entire poem itself, or even a whole book of poems. This block kick-starts the whole process of building blocks to put on the chain. 

Each time we need to create a new block, we call the nextBlock() function. 
<img src="https://violarisgeorge.files.wordpress.com/2017/10/wp3.png" alt="wp3" width="668" height="284" class="alignnone size-full wp-image-88240" />

This function creates the parts of a block, hashes their string outputs and adds the block parts along with their hash in a Block struct. 

In order to automate the blockchain generation process, I created a simple loop that creates and adds the blocks into a blockchain list.
<img src="https://violarisgeorge.files.wordpress.com/2017/10/wp4.png" alt="wp4" width="679" height="505" class="alignnone size-full wp-image-88241" />

Download gopherchain.go from <a href="https://github.com/violarisgeorge/gopherchain/blob/master/gopherchain.go">Github</a> and play with the code to create different kinds of blocks.
