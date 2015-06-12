##### Author : Michaël Lambé (mic0331 at gmail dot come)
## Summary
### Which country has the highest tax rate?
In which countries do high earners pay the most tax? And where do average earners pay the most?
Income tax has been a political hot potato for decades. 
This [visualization](https://radiant-basin-3159.herokuapp.com/) show the differences between countries for the earnings and taxes (without including the social security contributions - national insurance).
At the top end of the distribution we have Belgium where single people pay 33% of earnings in income tax, followed by Germany with 24%.

It is difficult to compare tax rates. Income tax is only one tax - most of us will pay other kinds of tax, like social security, and those with children might get some tax relief.  Here we only consider single person without children.

In a lot of the European countries tax rates and social security contributions are high but the provision of benefits by the state tends to be very generous compared to countries in other parts of the world.
If you fall ill or become unemployed the state will contribute and there are also generous pension arrangements.

*Note: some countries was missing data for some year. The choice has been made to replace the missing value by the mean of the taxes or earnings from all years.*
## Design
As the data are available on a yearly basis it was making sence to display a line chart to express the evolution/trend of the tax/tearning from year to year.
For an individual year, the pie chart on the top show percentage in a nice way so the reader can immediatly see the ratio of tax vs. earnings.
When the user mouve over a dot of the line graph, the pie chart display the corresponding ratio of the selected year.  
The choice has been made to **NOT** show the y axis to avoid an overload of information on the screen.  The user interested by a specific value of the line graph can get it when a cicrle is selected.
User can select a country by selecting an item in a combo box where an event trigger a callback to the server to retreive the needed data and refresh the charts.
The charts are using the same base color (`hex : #403075`), a ligher and darker version of this color is used to show respectively the earnings and taxes.
## Feedback
For collecting feedback, a dedicated [survey](https://www.surveymonkey.com/s/38DTMPD) has been conducted.
The following questions where asked :
* What do you notice in the visualization?
* What questions do you have about the data?
* What relationships do you notice?
* What do you think is the main takeaway from this visualization?
* Is there something you don’t understand in the graphic?
* How would you rate the quality of the visualization ? from very good to very bad
* Do you have any other comments, questions, or concerns?

The survey (conduted for a week between the 12th of June till the 19th of June)has been posted on various channel (udacity forum, reddit, twitter) in order to get a maximum feedback.
Overall, xxx persones reply and share a ...  TODO

## Technological choices
The web-based visualization has been implemented using an API developped for this project.  
The technology used are **mongodb, node.js, python, html, css and D3.js**.
The mini-project is hosted on [heroku](https://www.heroku.com/) and the database on [mongolab](https://mongolab.com/).  Both cloud solution are offering a free tier ideal for this proof-of-concept type of project.

The project source code folder contain two main area of interest.

1. **Preprocessing**
This is where the raw data material from [eurostat](http://appsso.eurostat.ec.europa.eu/nui/show.do?dataset=earn_nt_net&lang=en) are stored (`/proprocessing/data/earn_nt_net.tsv`). The file `preprocessing/preprocessor.py` is used to load the initial data, manipulate them and eventually perform some cleaning.  
The data was initialy coming from a TSV format and they were converted into JSON. `preprocessor.py` is also responsible to upload the data in mongodb where they will be consumed by the webapp.

2. **Webapp**
The root directory contain the webapp developed using the `express.js` framework. The structure of the webapp is very simple to understand.  An `server/api` folder contains endpoints consumed by the webpage.  For this project only the eurostat endpoint is used.  The folder `public` is where the webpage magic is happening.  The core of the implementation of the d3.js logic is in the file `public/app/app.js`.

For the reader interested to run the project locally, he should make sure mongodb, node.js and python3 are installed locally.
Next, NPM package manager should also be installed with the three global 
packages `gulp`, `nodemon` and `bower`.

To run the project, follow these steps :

1. Clone the repository

2. Make sure mongodb is running on default port and a db is created with the name `eurostat`.

3. In the folder `proprocessing` run 

    `>>>  python preprocessor.py`
    (will load the data in mongodb)

4. In the folder `webapp` run the command

    `>>>  npm inslall`
    (will install the local packages needed by the backend)

    `>>>  bower install`
    (will install the local packages needed by the frontend)

    `>>>  nodemon server`
    (this will start the webapp)

5. Navigate to `http://localhost:3030/`

6. The api can be access at `http://localhost:3030/api/v1/eurostat/basic/country/BE` where the final parameter can be changed according to the country of interest.

7. [OPTIONAL] During the devlopment phase of this project, `browsersync` has been used in order to have immediate feedback on the screen when a code modication was made. To run the project in development mode, simply run :

    `>>> gulp`

Then navigate to `http://localhost:4000/`

## Conclusion
Finding an interesting data set and a story it tells can be the most difficult part of producing an infographic or data visualization.

Data visualization is the end artifact, but it involves multiple steps – finding reliable data, getting the data in the right format, cleaning it up (an often underestimated step in the amount of time it takes!) and then finding the story you will eventually visualize.

## Resources
* [Eurostat - Net earnings and tax rates (earn_net)](http://appsso.eurostat.ec.europa.eu/nui/show.do?dataset=earn_nt_net&lang=en)
* [Eurostat - Net earnings and tax rates (metadata)](http://ec.europa.eu/eurostat/cache/metadata/en/earn_net_esms.htm)
* [programming - how to use d3.json](https://gist.github.com/mbostock/3750941)
* [programming - context and d3.json](http://stackoverflow.com/questions/30780654/how-to-properly-control-the-context-when-using-d3-json-event-handler/30780795?noredirect=1#comment49612132_30780795)