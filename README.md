# psuedogene-emailer
Develop a service to identify customers with a specified psuedogene and email them.

We have a database of our customers' DNA sequences.  To keep things simple for this exercise, we’ll simplify our customers' biology. Treat a DNA sequence as a string of characters.  Each character is one in the set of: “A”, “T”, “C”, or “G”.  Biologists call a character a _nucleotide_ and each nucleotide pairs up with another nucleotide to form a strand of DNA.  Human DNA has around 3 billion of these nucleotide pairs.  

We have over 10 million customers.  Like you, our customers have genes that allow us to create the proteins that make us up.  Genes only take up a small proportion of our DNA.  Something that takes up some of the rest of the room in a DNA sequence are _psuedogenes_. A pseudogene is like a gene but it doesn’t end up creating any proteins. Sometimes these psuedogenes turn out to be real genes, or otherwise important.

From time to time, we want to be able to send out bulk emails to customers who have a specified pseudogene that can be found in their DNA sequence.  The emails  will be drafted by our marketing department in a content management system.  

## Your mission
Create a working solution for this service.  Your solution must search through a database for any customers that have a given psuedogene and email them the given content.  At our next discussion, we will want to go through your solution with you to understand why you made the decisions you did.

> Don't spend more than 5 hours on this.  The fundamental rule is we want enough from you to have a conversation about the choices you made and why you made them.   > We want to know how you think.  This isn't a test.

## Requirements and additional information
1. The relational database has the following, very simple data structure where customers have multiple ordered DNA sequences:
```
┌───────────────────────────────┐     ┌──────────────────────────────────┐
│Customers                      │     │DNA                               │
├───────────────────────────────┤     ├──────────────────────────────────┤
│PK id          INTEGER NOT NULL│1─┐  │ PK id           INTEGER NOT NULL │
│   email_addr  CHAR(100)       │  └─*│ FK customer_id  INTEGER NOT NULL │
│   first_name  CHAR(50)        │     │    sequence     CHAR(2^16)       │
└───────────────────────────────┘     │    order        INTEGER NOT NULL |
                                      └──────────────────────────────────┘
```
PK denotes a unique primary key, and FK a foreign key.  If you want any test data, random DNA sequences can be created at https://www.bioinformatics.org/sms2/random_dna.html

2. The (simplified) method for identifying a psuedogene is:
```
Given the following inputs:

dna: A string of characters, that are expected to be in the set of “A”, “T”, “C”,
     or “G” (the possible nucleotides), representing a DNA sequence.
     
start_codon: A string of 3 characters representing the 3 nucleotides that 
             identify the start of a psuedogene. The characters are 
             expected to be in the set of “A”, “T”, “C”, or “G”
             
stop_codon: A string of of characters representing the 3 nucleotides that
            identify where the psuedogene ends. The characters are 
            expected to be in the set of “A”, “T”, “C”, or “G”

A psuedogene is found by:
   searching dna for the characters between start_codon and stop_codon. 
   If start_codon is found, then a valid psuedogene must contain at least
   three nucleotides and no start_codon.
   
For example:
   dna = CTATGGGATGGCATTGAT
   start_codon = ATG
   stop_codon = TTG
   The psuedo gene is GCA
```

3. You should send emails using SMTP to an external SMTP service.  You could use a Mock SMTP server like [MailHog](https://github.com/mailhog/MailHog) or [Mailslurper](https://www.mailslurper.com/).  You could also just simply mock this in your code.   

1. The content of the email is provided to you as a string. If the string contains the substring `{first_name}` then it must be replaced by the customers `first_name` value in the database.

1. Your code must be able to be invoked over HTTP using a tool like `curl` or [Postman](https://www.postman.com/). You can decide on how the Request and Response work

1. It is important that the every customer, who should be sent an email, is sent one, and emails are only sent to those customers.  The service should coded to be reliable, i.e. the mean-time between failure should be high.  You can assume the database is replicated in a high availability cluster and the SMTP service has a backup server that can be used(e.g. smtp.example.com and smtp2.example.com)

1. This service could be called at any time and possibly called while it is already busy with a previous request.

1. Preferably any code would be written in Javascript/Typescript/SQL but if you have more familiarity with another language, then you absolutely go and use that.

1. It would be nice if your submission include a README.MD file containing things like:
   - Instructions to build and deploy your solution, including any dependancies you require. 
   - Any assumptions you have made. 
   - How you suggest the solution is deployed to handle a large amount of requests and be highly available.
   - Documentation to developers on how to use the service
   - Any suggested improvements to make it more maintainable, performant, or otherwise "better"




