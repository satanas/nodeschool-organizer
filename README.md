# NodeSchool Assistant

Organizing a NodeSchool requires a lot of logistics; from finding dates, venue and food to updating the website to
reflect the upcoming event.

The goal of this assistant is to help you automate the tedious work of creating the event and updating the NodeSchool
website just by asking you some questions. Easy, right? Let's go to the details.

## Installation

```
npm install -g nodeschool-assistant mustache stylus
```

## Setup (Convention Over Configuration)

To make our lives easier, NodeSchool Assistant follows the Convention-over-Configuration approach. So, it does the
following assumptions:

1. You have a `.env` file in your project where the configuration is stored (check the [Environment Variables](#environment-variables)
section for more details about the values required).
1. Your NodeSchool website is hosted on Github (using Github Pages).
1. The folder used as output for your HTML files is `docs`.
1. You use `mustache` and `stylus` to generate your website (this will be eventually more flexible).
1. The `docs-src` folder contains the sources for your website. There, you must have:
  * `data.json` file: stores the data for your website (if it doesn't exist, it will be created after generating an event).
  * `images` folder: holds your images (e.g. the social image generated by the assistant and any other image used by your website).
  * `styles` folder: holds your Stylus files (.styl).
  * `templates` folder: stores all the templates to generate your website. In there, you must have:
    * `event-description.mustache`: The template to create the event description (used in Meetup.com, for example).
    * `index.mustache`: The template to generate your website's index.html.
    * `mentor-registration-issue.mustache`: The template to create the Github ticket used for mentor registration.
    * `social.mustache`: The template to generate the HTML that will render your social image.

## Environment Variables

These are the values required in the `.env` file inside the directory where your website lives:

 * **CHAPTER_NAME**: The default name of your chapter (e.g. "NodeSchool Seattle")
 * **GITHUB_ORG**: The github organization of your NodeSchool chapter (usually "nodeschool")
 * **GITHUB_REPO**: The github repo where you host the website for your chapter (e.g. "seattle")
 * **GITHUB_API_TOKEN**: Your personal github API token
 * **GITHUB_API_USER**: Your github username
 * **MEETUP_API_KEY**: Your Meetup.com API key
 * **MEETUP_URLNAME**: The URL Name of your group on Meetup.com (e.g. "Meetup-API-Testing")
 * **MEETUP_GROUP_ID**: The group ID of your group on Meetup.com (e.g. 1556336)
 * **BING_MAPS_API_KEY**: Your Bing Maps API key (to geolocate the venue during event creation)
 * **GITHUB_REMOTE**: (optional) The remote where you want to push when publishing (default: origin)
 * **GITHUB_BRANCH**: (optional) The branch you want to push when publishing (default: master)
 * **SOCIAL_IMAGE_WIDTH**: (optional) The desired width for your social image (default: 1200)
 * **SOCIAL_IMAGE_HEIGHT**: (optional) The desired height for your social image (default: 630)

## Usage

You can use the NodeSchool Assistant via the `nsa` command.

### Create event
```
nsa create-event --provider=meetup
```
The NodeSchool Assistant will ask you a series of questions regarding the new event (name, location, etc), update
the `data.json`, regenerate the website and publish the new version pushing the changes to your Github repo. You can
use the flags `--no-publish` and `--no-build` to avoid publishing or updating the website (although it's not recommended).

### Build website
```
nsa build-website
```
Useful if you manually edit the sources of your website and want to regenerate the HTML and the social image. Use it in
conjunction with `publish-website` to upload a new version of the website to Github.

### Publish webste
```
nsa publish-website
```
In case you manually edit your HTML or built a new version (with `build-website`) and want to publish the changes.


## TO-DO
If you want to contribute, here are a couple of things you might want to look at:
- Put venue address in the github ticket and remove link to meetup.com event
- Upload featured image to Meetup.com event
- Fix lat,lng (location) when creating on Meetup.com
- Improve data.json (add all the answers to the questions and create the file if it doesn't exist)
- Add support for multiple event providers (for example, Eventbrite)
- Add unit tests
- Add input validations and default values
- Improve config (only store base path and let each module handle the specifics)
- Add flexible pipeline to generate website (like allowing custom commands) instead of hardcoding mustache and stylus.
Maybe implementing something like a "recipe" or a list of bash commands that will be executed and must be defined somewhere
in the source directory or just running something like `npm run build`
