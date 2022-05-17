Activity (iam-2-cross-site-scripting-xss-)
Activity: XSS Attack!
In this activity we will get a chance to mock a cross-site scripting attack on our own code!
The activity will involve launching two distinct attacks on two distinct parts of the code.
However, because React is a very modern framework with modern defense measures,
we'll have to make some modifications to our code to make it vulnerable.

Setup
Instructions:
Fork (not clone) it to your OWN GitHub account.
Now to clone the repo to your machine, click the green 'Code' button and then copy the URL.
In a new terminal, or Git Bash, go to where you want to clone the repo.
Type git clone in the terminal or Git Bash, then a space, then paste the URL you copied from your repo. Example:
undefinedClick here to copy
Hit "Enter" or "Return" whichever is on your keyboard.
In your terminal, run npm install to install the required node_modules.
After the install, run npm start in the terminal.
Do the assignment in Visual Studio Code and stage your changes using git add -A command.
Make at least one commit by using git commit -m "write your message here" command. Example:
undefinedClick here to copy
Finally push your changes using the git push command. Example:
undefinedClick here to copy
First Attack!
Once we've opened our small task list tracker in the browser, we can begin the process of hacking ourselves!
Let's try to attack our code before we make any changes. Go ahead and submit a task with a random image URL and the following text: <script>alert("Hacked!")</script>
Investigate what happened. Does it seem like we made an impact on the page? No? Okay, let's try something else.
Let's return to our codebase and navigate to App.js.
Insert the highlighted line below into the handleSubmit function. eval() is a dangerous React function that will convert any argument it receives to JavaScript. We're only using it in this case so that we can make our code vulnerable to an XSS attack. React highly discourages the use of the eval() function.
undefinedClick here to copy
Next, in the browser, let's head to the show page of one of the places stored in our database.
Submit a comment with the text: alert("hacked!")
What happened? You should have gotten a browser alert with the text "hacked!"
If this had been a real vulnerability, we could have submitted any amount of JavaScript code as a comment, and it would have run without a problem. This sort of simplistic attack is not much of a threat to the modern web, thanks to built-in security measures and data sanitization that is carried out automatically. However, when the internet was mostly comprised of HTML, such attacks could have wreaked havoc on unsuspecting sites.

Second Attack!
At one time, images posed a gigantic security threat for websites. Let's do a little hacking to make our codebase vulnerable to an image scripting attack.

In App.js, navigate to the bottom of the file.
Insert the highlighted line of code below. In order to complete this attack, we will include the dangerouslySetInnerHTML React property. As the name suggests, it will convert whatever is set equal to the _html key to HTML without asking any questions. This creates a massive vulnerability in our code.
undefinedClick here to copy
Get ready to submit a task by writing anything into the text area.
Before submitting, add this line of code into the image input field:
<img src="1" onerror="alert('Gotcha!')" />
Note the onerror prop that we included with this image tag. This equates to HTML directions for what to do if the browser has a problem displaying the image. Since the image src="1" doesn't exist, the onerror will fire as soon as the image tag is read by the browser.
Observe what happened. As soon as we entered the img tag into the field, we received the alert! This is because of how state works in React. As soon as our malicious img tag was set equal to task.image, it became HTML within our dangerous hidden div. Finally, as soon as it became HTML, the onerror did its job, unwittingly running our attack for us!

eval(task.text);
const handleSubmit = e => {
    e.preventDefault()
    if (task.text || task.image) {
      tasks.push({
        text: task.text,
        image: task.image
      })
      setTask({
        text:"",
        image:""
      });
      eval(task.text);
    }
  }

  document.getElementsByTagName('body')[0].remove();

  ...
       <h2>Tasks on your list:</h2>

{tasks.map((task, index) => (
  <Task
  text={task.text}
  image={task.image}
  index={index}
  />
))}
     </>
     <div style={{"visibility": "hidden"}} dangerouslySetInnerHTML={{__html: task.image}} />
     </>
   )
 }
 ...

 <img src="1" onerror="alert('Gotcha!')" />


