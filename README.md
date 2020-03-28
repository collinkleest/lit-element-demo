![lit-element](https://dev-to-uploads.s3.amazonaws.com/i/n6kmj4f3s7vf8sagiib3.png)

Welcome to my first "dev.to" post, in this post I am going to be going through the basics of building a web component with Lit Element. I will be creating a project card html element that can be reused to list mutiple projects. This is great if you have a personal portfolio with a projects page. 

Youtube Video Tutorial: https://www.youtube.com/watch?v=8gMDhXWMxRg&t=2s
Github Repo w/ Code: https://github.com/collinkleest/lit-element-demo/

## What is a Web Component and what is Lit Element?
**Web Component:** a web component is a custom html tag that encapsulates HTML, CSS, and Javascript functionality into a single html tag. Web components work across modern browsers and work inside any web framework (e.g. react, vue, or angular). Web Components use the shadow document object model (DOM) which allow components to be excluded from global css styling, therefore components have their own styling defined inside the component. 

**Lit Element:** lit element is base class for creating web components, it is backed by the polymer project web framework which is being worked on by google developers. Web components can be developed "natively" by extending HTMLElement but Lit Element makes it much easier for developers to create components. 

## Lets Build a Web Component!

### Installation
I am currently on Fedora a Red Hat Linux distribution so the installation of everything will be tailored towards Linux users but the same can be done on Mac and Windows with a little bit of extra googling. Lets get started!

Lets first install nodejs, when you install node it will also include the node package manager (NPM). You can find the download here [NodeJS]('https://nodejs.org/en/download/').

On Fedora you will need to have the RPM Fusion repositories for easy installation. Go ahead and install the free and non-free RPM fusion repos.
Free:
```bash
$ sudo dnf install \
  https://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm
```
Non-free (optional):
```bash
$ sudo dnf install \
  https://download1.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm
```
We are going to install the latest stable version, you can install the newest cutting edge version if you desire to. 
Install nodejs and npm:
```bash
$ sudo dnf -y install nodejs
```
Make sure the installation was successful by checking the version. 
```bash
$ node --version
v12.16.1
```
Check if NPM was successful as well.
```bash
$ npm --version
6.14.4
```
Now we are going to install polymer but first since we have a fresh installation of node we need to make sure we have sufficient permissions to the node_modules folder usually in the `/usr/local/lib/node_modules` directory.
Perform the following command:
```bash 
$ sudo chown -R $USER /usr/local/lib/node_modules
```
Now that we have permissions we are going to use NPM to install polymer-cli, use the following command and check the version to make sure it was installed correctly.
```bash
$ npm install -g polymer-cli
$ polymer --version
1.9.11
```
The last step for getting setup is 1) creating a folder for our project, 2) creating a JavaScript file (`projectCard.js`) and an html starter point (`index.html`) 3) installing lit-element in the current project directory. Use the following commands to do so:
```bash
$ mkdir lit-element-demo
$ touch index.html projectCard.js
$ npm install lit-element
```
Lets start with the HTML, here I will list the most important things needed. We must link our JavaScript file to our HTML file, we can do this with the following HTML script tag: 
```html 
<script type="module" src="projectCard.js"></script>
```
Even though we haven't written any code for our web component lets define it in our html. Our component which will be called `<project-card>` will have three attributes; name, url, and img. The name attribute will be the name of our project, the img attribute will be a link to an image we want the card to display and the url is a direct link to the project. 
```html
<project-card name="Python Automation" img="https://img.icons8.com/nolan/300/python.png" url="https://github.com/collinkleest">
</project-card>
```
For reference here is the complete HTML.
#### Complete HTML
```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Lit Element Demo</title>
    </head>
    <body>
        
        <project-card name="Python Automation" img="https://www.stickpng.com/assets/images/5848152fcef1014c0b5e4967.png" url="https://github.com/collinkleest/GitHub_CLI_Automator"></project-card>
        <script type="module" src="projectCard.js"></script>
    </body>
</html>
```

Now lets go over to our JavaScript file, I am going to highlight some of the key parts of building a Web Component.
First we must import lit-element at the top of our file, then we will need to create a class, and lastly we will inject our element into the document with `customELements.define('element-name', Element-Class);`.
```js
// import LitElement, HTML, & CSS from our lit element module
// when Polomer deploys it fill fill the path for our lit-element module
import {LitElement, html, css} from 'lit-element';

//this is our class where everything that goes into our element will be
class ProejctCard extends LitElement{

}

// this is where we "inject" our custom element into our HTML file
customElements.define('project-card', ProjectCard);
``` 

Inside our class lets define two methods, a get method for our properties aka (class variables, attributes) and an empty constructor. The empty constructor is used if someone creates an element without passing in attributes. 
```js
constructor(){
        super(); //calls the empty LitElement constructor
        this.name = "Project Name";
        this.url = "https://google.com";
        this.img = "https://img.icons8.com/cotton/2x/folder-invoices.png"
    }
    
    static get properties(){
        return{
            name: { type: String },
            url: { type: String},
            img: { type: String}
        };
    }

```
Next, lets call the render function which actually renders our HTML. Make sure you are still in the ProjectCard class.
```js
render(){
        return html`
            <div class="card">
                <img src="${this.img}" width=300px>
            
                <div class="container">
                    <h3> <b> ${this.name} </b> </h3>
                    <p> <a href="${this.url}"> Project Link </a></p>
                </div>
            </div>
        `;
    }
```
Lastly add a get styles method for CSS styling.
```js
static get styles(){
        return css`
            .card{
                box-shadow: 0 4px 8px 0 rgba(0,0,0,0.2);
                transition: 0.3s;
                border-radius: 5px;
                width: 300px;
            }
            .card:hover{
                box-shadow: 0 8px 16px 0 rgba(0,0,0,0.2);
            }
            .container  {
                padding: 2px 16px;
            }
        `;
    }
```

#### Complete `projectCard.js` file.
```js
import {LitElement, html, css} from 'lit-element';

class ProjectCard extends LitElement{
    // defined css styling
    static get styles(){
        return css`
            .card{
                box-shadow: 0 4px 8px 0 rgba(0,0,0,0.2);
                transition: 0.3s;
                border-radius: 5px;
                width: 300px;
            }
            .card:hover{
                box-shadow: 0 8px 16px 0 rgba(0,0,0,0.2);
            }
            .container  {
                padding: 2px 16px;
            }
        `;
    }
    
    
    render(){
        return html`
            <div class="card">
                <img src="${this.img}" width=300px>
            
                <div class="container">
                    <h3> <b> ${this.name} </b> </h3>
                    <p> <a href="${this.url}"> Project Link </a></p>
                </div>
            </div>
        `;
    }
    
    constructor(){
        super();
        this.name = "Project Name";
        this.url = "https://google.com";
        this.img = "https://img.icons8.com/cotton/2x/folder-invoices.png"
    }
    
    static get properties(){
        return{
            name: { type: String },
            url: { type: String},
            img: { type: String}
        };
    }

}

customElements.define('project-card', ProjectCard);
```


We are done! Make sure you save your files and go to your command line or terminal to deploy with polymer!

Use the following command to deploy with Polymer!

```bash
$ polymer serve

#EXPECTED OUTPUT:

info:	Files in this directory are available under the following URLs
      applications: http://127.0.0.1:8081
      reusable components: http://127.0.0.1:8081/components/lit-element-demo/
```
Use that first link to view your web component in action.
For me it was `http://127.0.0.1:8081` or `http://localhost:8081`.
 
#### Finished Productt!
![Finished Product](https://dev-to-uploads.s3.amazonaws.com/i/wzkuv0o5ludialfh5f35.png)

### Additional Resources / Links
- [Youtube Video](https://www.youtube.com/watch?v=8gMDhXWMxRg&t=2s)
- [Github Repo](https://github.com/collinkleest/lit-element-demo)
- [Polymer Reference](https://www.polymer-project.org/)
- [Webcomponents Reference](https://www.webcomponents.org/)
- [LitElement Reference](https://lit-element.polymer-project.org/)

### Social Media
- [LinkedIn](https://linkedin.com/in/collinkleest)
- [Github](https://github.com/collinkleest)
