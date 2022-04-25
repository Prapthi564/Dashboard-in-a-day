![Microsoft Power Platform.](Images/powerbi-welcome-7.png 'Microsoft Power Platform')

# Lab 7 - Embedded Power BI App in Web App

**Contents** 

<!-- TOC -->

- [Introduction](#introduction)
- [Embedded Power BI](#embedded-power-bi)
- [References](#references)

## Introduction

**Embedded analytics** and **Power BI Embedded** (the Azure offer) allow you to embed Power BI content such as reports, dashboards and tiles, into your application.

In this lab, you'll learn how to:

 - Set up your embedded environment.
 - Configure an embed for your customers (also known as app owns data) sample application.

To use application, users won't need to sign in to Power BI or have a Power BI license.

We recommend using the embed for users method to embed your Power BI content, if you're an independent software vendor (ISV) or a developer, who wants to create applications for third parties.

### Code sample specifications

This lab includes instructions for configuring an embed for sample application in one of the following frameworks:

 - .NET Framework
 - .NET Core
 - Java
 - Node JS
 - Python

The code samples support the following browsers:

 - Microsoft Edge
 - Google Chrome
 - Mozilla Firefox


## To create an embed for users sample app, follow these steps:

### Task 1 - Select your authentication method

Your embedded solution will vary depending on the authentication method you select. Therefore, it's important to understand the differences between the authentication methods, and decide which one best suits your solution.

The table below describes a few key differences between the [service principal](https://docs.microsoft.com/en-us/power-bi/developer/embedded/embed-service-principal) and **master user** authentication methods.

   | Consideration            | Service principal             | Master user             |
   | ------------------------ | ----------------------------- | ----------------------- |
   | Mechanism                | Your Azure AD app's [service principal object](https://docs.microsoft.com/en-us/azure/active-directory/develop/app-objects-and-service-principals#service-principal-object) allows Azure AD to authenticate your embedded solution app against Power BI.                              | Your Azure AD app uses the credentials (username and password) of a Power BI user, to authenticate against Power BI. |
   | Security                         | Service principal is the Azure AD recommended authorization method. If you're using a service principal, you can authenticate using either an application secret or a certificate. This tutorial only describes using service principal with an application secret. To embed using a service principal and a certificate, refer to the [service principal with a certificate](https://docs.microsoft.com/en-us/power-bi/developer/embedded/embed-service-principal-certificate) article.                              | This authentication method isn't as secure as a service principal. You have to be vigilant with the master user credentials (username and password). For example, don't expose them in your embedding application, and change the password frequently. |
   |                          |                               |
   |                          |                               |






















