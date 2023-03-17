---
title: "Landing page for your app"
date: 2020-08-14
draft: false
description: "How to create beautifull landing page for your IOS app"
slug: "LandingPage"

---

## Simple website for your app

Ok you have published your beautifull app on the App Store or maybe you are thinking to publish your app after a TestFlight period. Maybe you need a place for your privacy and policy document, or a place where you can track all the changelogs. A website is the perfect solution

## What you need

In order to make things easier as possible, we will use the potentiality of github pages, a service that will host our website with also a domain. So make sure you have a github account and a little expirience with git


## How it works

This is a static website built with the Jekyll framework. So what jekyll does is basically build a static website and the user can apply a theme or a template, so you don't need to code nothing. For this project you will use an already existsting template. You can go on this [repository](https://github.com/emilbaehr/automatic-app-landing-page.git)

## What you have to do

Firs step, fork the repository in order to have the same repository on your account. Once you have forked the repository make sure you have enabled github pages on this repo. So go in setting, in the lateral bar find Pages and make sure you have enabled. Now github will build and host will build and host your website without any problem.

## Cool feature

Clone the repo and open it with your preferred text editor. The most interesting thing is that if you paste into the _config.yaml page your ios_app_id it grabs automatically the name, the icon, the price and other stuff. If this doesn't work you can modify the aspect from the _config.yaml. In case you want to add your privacy and policy terms or your changelog, there are two markdown files in the _pages folder that you can modify.


