# Build-a-Resume-on-AWS
![resume drawio](https://github.com/Soumodip40/Build-a-Resume-on-AWS/assets/128739966/a667b34f-4f4b-42ca-b6a8-edc73a258d2b)

## First step
Creating HTML, CSS and JavaScript files—with the help of ChatGPT.
# index.html file

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Online Resume - Your Name</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <div class="container">
        <header>
            <img src="headshot.jpg" alt="Headshot" class="headshot">
            <h1 id="name">Name</h1>
            <p id="contactInfo">Location | Email</p>
        </header>
        <section id="employmentHistory">
            <h2>Employment History</h2>
            <div id="timeline"></div> <!-- Placeholder for the JavaScript array -->
        </section>
        <section id="education">
            <h2>Education</h2>
            <ul>
                <li>Degree | University (Year)</li>
                <li>Degree | University (Year)</li>
                <!-- Add more list items as needed -->
            </ul>
        </section>
        <section id="skills">
            <h2>Skills/Certifications</h2>
            <ul>
				<li>Skill 1</li>
				<li>Skill 2</li>
				<li>Skill 3</li>
				<li>Skill 4</li>
                <!-- Add more list items as needed -->
			</ul>
        </section>
    </div>
    <script src="script.js"></script>
</body>
</html>
# styles.css file
/* Base reset for padding and margin for all elements */
* {
    padding: 0;
    margin: 0;
    box-sizing: border-box;
}

/* Body styling */
body {
    font-family: 'Arial', sans-serif;
    line-height: 1.6;
    background-color: #f4f4f4;
}

/* Container for centering the content */
.container {
    width: 80%;
    margin: auto;
    overflow: hidden;
    padding: 20px;
}

/* Header styling */
header {
    background: #333;
    color: #fff;
    padding: 20px;
    text-align: center;
}

/* Header image and name styling */
.headshot {
    width: 150px;
    height: 150px;
    border-radius: 50%;
    display: block;
    margin: 20px auto;
}

header h1 {
    margin-bottom: 10px;
}

/* Contact information styling */
#contactInfo {
    font-size: 1.1em;
    margin-bottom: 20px;
    color: #fff; 
    padding: 15px;
}


/* Section styling for employment, education, and skills */
section {
    background: #fff;
    margin: 20px 0;
    padding: 15px;
    border-radius: 8px;
    box-shadow: 0 2px 4px rgba(0,0,0,0.1);
}

section h2 {
    margin-bottom: 10px;
}

/* Timeline styling */
#timeline .entry {
    border-left: 3px solid #333;
    margin-bottom: 5px;
    cursor: pointer;
}

#timeline .entry-header {
    background: #e2e2e2;
    padding: 10px;
    margin-left: -3px; 
}

#timeline .entry-header:hover {
    background: #ccc; 
    color: #333; 
}

/* Style for the job description content */
#timeline .entry-content p {
    padding: 5px 10px;
    background: #f9f9f9;
    border-left: 3px solid #333;
    display: block; 
}

/* List styling for education and skills */
section ul {
    list-style: inside square;
    padding: 0 20px;
}

section ul li {
    padding: 2px 0;
}

/* Adjustments for active class */
.entry.active .entry-header {
    background-color: #e2e2e2; 
    color: #333; 
}

.entry.active .entry-content {
    display: block; 
}

/* Visual cue for clickable items */
.entry .entry-header:after {
    content: ' (click to expand)';
    font-size: 0.8em;
    color: #666;
}

.entry.active .entry-header:after {
    content: ' (click to collapse)';
    font-size: 0.8em;
    color: #666; 
}
# script.js file
// Used on the résumé to make the employment history interactive (each job is clickable)
document.addEventListener('DOMContentLoaded', function () {
    // Placeholder array with employment history data
    const employmentHistory = [
        { id: 1, title: 'Job Title', company: 'Company Name', years: 'Year - Year', description: 'Description of what you did' },
        { id: 2, title: 'Job Title', company: 'Company Name', years: 'Year - Year', description: 'Description of what you did' },
        { id: 3, title: 'Job Title', company: 'Company Name', years: 'Year - Year', description: 'Description of what you did' }
        // Add more entries as needed
    ];

    const timeline = document.getElementById('timeline');

    // Create timeline entries
    employmentHistory.forEach(job => {
        // Entry container for job
        const entry = document.createElement('div');
        entry.className = 'entry';
        entry.id = 'entry-' + job.id;

        // Title header for job
        const header = document.createElement('div');
        header.className = 'entry-header';
        header.innerText = job.title;

        // Content container for job, initially hidden
        const content = document.createElement('div');
        content.className = 'entry-content';
        content.innerHTML = `<strong>Company:</strong> ${job.company}<br>
                             <strong>Years:</strong> ${job.years}<br>
                             <p>${job.description}</p>`;
        content.style.display = 'none';

        // Append header and content to the entry
        entry.appendChild(header);
        entry.appendChild(content);

        // Event listener to toggle content visibility
        header.addEventListener('click', function () {
            // Check if the clicked header's content is currently shown
            const isContentShown = content.style.display === 'block';
            // Hide all open contents
            document.querySelectorAll('.entry-content').forEach(el => {
                el.style.display = 'none'; // Hide content
            });
            // Deactivate all headers
            document.querySelectorAll('.entry').forEach(el => {
                el.classList.remove('active'); // Remove active class
            });

            if (!isContentShown) {
                // If it was not shown before, display it
                content.style.display = 'block';
                entry.classList.add('active');
            } // If it was shown, it will be hidden as part of the above loop
        });

        timeline.appendChild(entry);
    });
});
## Second step Create an S3 Bucket and Configure it for Static Website Hosting and Public Access

Now that you have your three code files (plus don't forget a "headshot.jpg" to display your smiling face), you need somewhere to put them.  
In AWS, S3 is a great option for inexpensive object (read: files) storage. And if you're only using client-side code like you are, then you can configure S3 for static website hosting.

## Copy the following bucket policy (JSON code).
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": [
                "s3:GetObject"
            ],
            "Resource": [
                "arn:aws:s3:::Bucket-Name/*"
            ]
        }
    ]
}

# If everything worked, you should see your handiwork displayed in the browser.  Yay

## Register a New Domain Name with Route 53
If you don't already have a domain name, then you can register one with AWS, using Route 53. 

## Create an A Record with an Alias to Point to the S3 Website
Now that you have a public hosted zone, you need to create a record that says how traffic should be routed when someone goes to your domain name.

Click into your hosted zone, and then click Create record.

Now let's test!  If everything worked, then when you type your domain name into a browser (like amberaws.com), Route 53 should direct you to the S3 website, which means you should see your (awesome) Resume.


## Now need to Create a Public TLS/SSL Certificate using AWS Certificate Manager
If you need a refresher on certificates, these help ensure a secure connection between users and the server they're making a request to.  

If I'm sending bank information across the internet, for example, I want to know that it's going to a server that's reputable, and that the connection is encrypted. And even for something as "simple" as a resume, a secure connection will give viewers confidence that they haven't ended up on a sketchy website.


Before a certificate can be issued, Amazon needs to confirm that you own this domain and that you're able to modify DNS settings (in Route 53). To start this process, click Create records in Route 53.


# Great! You have a TLS/SSL certificate, but what do you do with it now?

# Your website files are currently hosted in S3, but unfortunately, you can't use a certificate on an S3 bucket.


## Create a CloudFront Distribution

CloudFront is Amazon's content delivery network, or CDN. It's used to get content to users faster by caching it at "edge locations" around the world. This works great for things like videos and images, making them faster to load.  

For your simple résumé, because the files are so small, you won't notice much of a performance difference. But it is the way you'll be able to apply the TLS/SSL certificate you created in the last section.

![Screenshot 2023-12-29 015633](https://github.com/Soumodip40/Build-a-Resume-on-AWS/assets/128739966/1e1a933a-cbe6-49e0-98c4-d89876a52871)


# It works.

# The resume files (coming from S3 via CloudFront) load on the custom domain name (from Route 53) over a secure connection using the TLS/SSL certificate (from Certificate Manager). Nice work.


