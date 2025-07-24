\documentclass[a4paper, 11pt]{article}          % Paper and font size
\usepackage[english]{babel}                     % Language setting

% Set margins
\usepackage[top=1.5cm,bottom=2.5cm,left=2cm,right=2cm,marginparwidth=1.75cm]{geometry}

% Load useful packages
\usepackage{graphicx}                           % Graphics
\usepackage{amsmath}                            % Math
\usepackage{xcolor}                             % Link color
\definecolor{custom-blue}{RGB}{0,99,166} 
\usepackage{hyperref}
\hypersetup{colorlinks=true, allcolors=custom-blue}
\usepackage[default]{sourcesanspro}             % Load font
\usepackage[T1]{fontenc}                        % Special characters
\usepackage{fancyhdr}                           % Load header, footer package

\usepackage{lastpage}                           % Footer note
\usepackage{lipsum}                             % For dummy text
\usepackage{minted}
\usemintedstyle{monokai}
\usepackage{xcolor} % to access the named colour LightGray
\definecolor{LightGray}{gray}{0.9}
% \pagestyle{myheadings}                          % Own header
% \pagestyle{fancy}                               % Own style
% \fancyhf{}                                      % Clear header, footer

\setlength{\headheight}{10pt}                   
\renewcommand{\headrulewidth}{0.5pt}            % Top line
\renewcommand{\footrulewidth}{0.5pt}            % Bottom line

\fancyfoot[C]{}                                 % Footer center
\fancyfoot[R]{\thepage/\pageref{LastPage}}      % Footer right

\begin{document}

\title{Tutorial: Calling a REST API in an Express App}
\author{Chris Kehoe - cbkehoe22@gmail.com}


\maketitle                                      % Include contents
\tableofcontents

\section{Introduction}                            
    Welcome to my tutorial on calling a REST API within an Express application. This tutorial was built with XML and LaTeX.\\
    \newline
    While there are several ways to implement an API call in an app, this tutorial will focus on making a simple endpoint call to an API using Node.js in an Express Framework and coded in Visual Studio Code.
\section{Install Node.js}

To install Node.js, download the file package from their website: \url{nodejs.org/en/download}.
Once the download is finished, open the file and install it using the recommended settings. Now that you have installed the node.js package, you can create an application to call a REST API. 

\section{Set Up Your Application with Express} % Example tables


To begin, let's create an express app. Open a computer terminal and input the following commands (be sure to press \verb|Enter/Return| after every terminal command):
\begin{minted}
[
frame=lines,
framesep=2mm,
baselinestretch=1.2,
bgcolor=LightGray,
fontsize=\footnotesize,
% linenos
]
{html}

$ mkdir ExpressAPICall
$ cd ExpressAPICall

\end{minted}
Initialize the app with the command:
\begin{minted}
[
frame=lines,
framesep=2mm,
baselinestretch=1.2,
bgcolor=LightGray,
fontsize=\footnotesize,
% linenos
]
{html}
$ npm init -y

\end{minted}
The purpose of the \textbf{-y} at the end of \textbf{npm init} is to automatically accept the application’s default settings, which will be sufficient for this tutorial.
\newline
\newline
Once the project has been initialized, install the Express framework with the following command:

\begin{minted}
[
frame=lines,
framesep=2mm,
baselinestretch=1.2,
bgcolor=LightGray,
fontsize=\footnotesize,
% linenos
]
{bash}
$ npm install --save express   
\end{minted}
\section{Set Up The Middleware}                         
Open the \emph{ExpressAPICall} project in Visual Studio Code, create a file called \emph{index.js}, and enter the following code:
\begin{minted}
[
frame=lines,
framesep=2mm,
baselinestretch=1.2,
bgcolor=black,
fontsize=\footnotesize,
linenos
]
{js}


const application 'require('express');
const app = express();
const port = 3000;


app.get('/', (req, res) => res.send('Skynet is Online'))
app.listen(3000, port)

\end{minted}
Then, enter the following command in your terminal: 
\begin{minted}
[
frame=lines,
framesep=2mm,
baselinestretch=1.2,
bgcolor=LightGray,
fontsize=\footnotesize,
% linenos
]
{bash}

$ node index.js

\end{minted}
Open a browser and navigate to \url{localhost:3000/}. If the message \textcolor{orange}{'Skynet is Online'} is displayed in the browser, then the app is running correctly. After you have confirmed that the app is running, return to the terminal window and type \verb|ctrl + c|
to exit the node console. 

\section{Connecting To An External Data Source}                     
The next step is to create a request module. Enter the following command into the terminal:

\begin{minted}
[
frame=lines,
framesep=2mm,
baselinestretch=1.2,
bgcolor=LightGray,
fontsize=\footnotesize,
% linenos
]
{bash}
$ npm install --save request

\end{minted}
Now that the Request Module has been installed, create a file titled \emph{restCall.js} and enter the following code:
\begin{minted}
[
frame=lines,
framesep=2mm,
baselinestretch=1.2,
bgcolor=black,
fontsize=\footnotesize,
linenos
]
{js}
const request = require("request");

module.exports = {
  call_API: function (url) {
    return new Promise((resolve, reject) => {
      request(url, { json: true }, (err, res, body) => {
        if (err) reject(err);
        resolve(body);
      });
    });
  },
};
\end{minted}

 \textit{\textcolor{gray}{{The above bit of code will return a promise that will be resolved or rejected by the API response.}}}
\newline
\newline
Return to the \emph{index.js} file and require the new module near the top of the file as seen below:

\begin{minted}
[
frame=lines,
framesep=2mm,
baselinestretch=1.2,
bgcolor=black,
fontsize=\footnotesize,
linenos
]
{js}
const express = require('express');
const restCall = require("./restCall"); //<----- require the module here
const app = express();
const port = 3000;
\end{minted}
\section{Make A Rest API Call}
   Now, all that remains is to refactor the GET request in \emph{index.js} to retrieve some dummy data from\\ \url{https://jsonplaceholder.typicode.com/todos/} as seen below:
\begin{minted}
[
frame=lines,
framesep=2mm,
baselinestretch=1.2,
bgcolor=black,
fontsize=\footnotesize,
linenos
]
{js}
const express = require('express');
const restCall = require("./restCall"); 
const app = express();
const port = 3000;

app.get("/", (req, res) => {
  restCall
    .call_API("https://jsonplaceholder.typicode.com/todos/")
    .then((response) => {
      res.json(response);
    })
    .catch((error) => {
      res.send(error);
    });
});

app.listen(3000, port)
\end{minted}
Once the GET request has been refactored, save all changes, restart the server, and navigate to\\
\url{localhost:3000/} to see the returned API response in the web browser.

\section{Conclusion}
This approach to making REST API calls can be adapted to work with a wide range of APIs. To retrieve specific types of data, consult the API’s documentation to identify the available endpoints and their required parameters.


\end{document}
