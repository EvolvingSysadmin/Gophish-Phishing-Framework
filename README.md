# Gophish Phishing Framework

![Let's Phish Bruh](/img/logo.png)

This repository contains instruction on the use of the Gophish phishing framework to evaluate information security.


## Topics
- [Background](#background)
- [Use Case](#use-case)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Usage](#usage)
  - [Run Gophish](#run-gophish)
  - [Access Gophish Administrative Page](#access-gophish-administrative-page)
  - [Change Account Settings](#change-account-settings)
  - [Create a Landing Page](#create-a-landing-page)
  - [Create Phishing Email](#create-phishing-email)
  - [Create Email Campaign](#create-email-campaign)
- [Resources](#resources)

## Background

Gophish is an open-source phishing toolkit written in Go for testing network security. Gophish makes it easy to execute phishing engagements. Gophish can create web pages that mimic genuine web pages with login forms. Gophish can then send emails with links to the phishing web pages and track user engagement.

## Use Case

Use Gophish to execute phishing campaigns to assess and increase network security. Gophish can identify the need for user awareness training, email antispam and content filtering, and anti-spoofing technologies.

## Prerequisites

Gophish requires that the Go programming language and a C compiler are installed on the Gophish administration server. The Go programming language can be installed from the [official Go download page](https://go.dev/dl/). A C compiler such as the GNU Compiler Collection (GCC) can be installed on Unix based operating systems from the [GNU Compiler Collection download page](https://gcc.gnu.org/releases.html). Gophish should be installed on a server that is exposed to the Internet. A Virtual Private Server running a Unix based operating system works well for deploying the Gophish administration server.

## Installation

Run the following command to install Gophish and download it into the `$GOPATH` directory from GitHub:

```
go get github.com/gophish/gophish
```

After the installation, navigate to `$GOPATH/src/github.com/gophish/gophish` and run the command `go build` to build a gophish binary in the directory.

Configure the `config.json` file located in the gophish root directory with the settings native to your server environment. The following options can be configured with relevant preferences:

| Key                      | Default Value  | Description                                                             |
| ------------------------ | -------------- | ----------------------------------------------------------------------- |
| admin_server.listen_url  | 127.0.0.1:3333 | IP/Port of gophish admin server                                         |
| admin_server.use_tls     | false          | Use TLS for admin server?                                               |
| admin_server.cert_path   | example.crt    | Path to SSL Cert                                                        |
| admin_server.key_path    | example.key    | Path to SSL Private Key                                                 |
| phish_server.listen_url  | 0.0.0.0:80     | IP/Port of the phishing server - this is where landing pages are hosted |

Change the entry for `phish_server.listen_url` to `0.0.0.0:3333` in order to expose the administration server to the Internet. Gophish creates a self-signed SSL certificate for the administrative panel by default.

Add the [service script](/scripts/gophish-service.sh) to the `/etc/init.d/gophish` directory to run Gophish as a service.

## Usage

### Run Gophish

Use the following command in the directory with the gophish binary to run Gophish:

```
sudo service gophish start
```

### Access Gophish Administrative Page

Navigate to the default server listening URL of `https://127.0.0.1:3333/` to access the Gophish administrative login page. Login to the administrative login page using the default credentials of:

```
Username: admin
Password: gophish
```

### Change Account Settings

Navigate to the "Settings" link of the administration website to update basic settings such as the administrative password.

### Create a Landing Page

The Gophish landing page is the HTML page that returns for targeted users when they click the links in the phishing emails they receive. Gophish landing pages support capturing credentials, email content templating, and website redirection after users submit credentials. 

Navigate to the "Landing Pages" link and click the "New Page" button to create a landing page. Landing pages can be created using an HTML WYSIWYG editor.

![Gophish Langing Page Creation](/img/landing-page.png)

Gophish landing pages can also be imported from existing websites. Click the "Import Site" button, enter the URL of the website to be imported, and click "Import". The HTML of the website URL that was imported should populate in the editor.

![Gophish Langing Page Import](/img/landing-page-import.png)

### Create Phishing Email

Gophish phishing emails are created as templates. Templates can be created from scratch or imported from existing email. 

Navigate to the "Email Templates" page and click the "New Template" button to create a template. An email template can be created using text or the HTML WYSIWYG editor. 

Gophishing email templates can also be created by importing existing email. Click the "Import Email" button and paste in the original email source content to import an existing email message.

![Gophish Email Template Creation](/img/template.png)

### Create Email Campaign

SMTP relay information must be configured to send Gophish emails. SMTP relay information is configured in "Sending Profiles." Click the "Sending Profiles" link in the sidebar of the Gophish administration site. Input the profile name, interface type (such as SMTP), valid sender's email address, SMTP hostname, and SMTP credentials. Click the "Send Test Email" button to test the SMTP configuration settings.

![Gophish Email Template Creation](/img/sending-profile.png)

Navigate to the "Campaigns" page in the Gophish administration site sidebar to send emails to targeted users. 

To configure and launch a campaign, click the "Campaigns" entry in the navigation sidebar. Configure the following options to send the emails of the campaign:

- Name: campaign name
- Email Template: email template to send campaign recipients
- Landing Page:  landing page to send campaign recipients
- URL: URL of the Gophish server
- Launch Date: date that the campaign will begin
- Send Email By: date campaign will be sent by
- Sending Profile: SMTP configuration of emails
- Groups: group of recipients to recieve the email

![Gophish Email Template Creation](/img/campaign.png)

Users are redirected to the campaign results screen when a campaign is launched. Expand the row with the recipient's name to view engagement information for that recipient. This results view will show engagement information, such as whether the recipient opened the email, clicked the link, or submitted data such as credentials to the landing page.

## Resources
- [Gophish Github Repository](https://github.com/gophish/gophish )
- [Gophish Documentation](https://getgophish.com/documentation/)