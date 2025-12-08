---
draft: false
date: 2024-07-21
category: "Portfolio"
authors:
    - jkoblitz
tags: [portfolio, website]
---

# The way to OSIRIS Portfolio

... and where it got its name from

Ever since we developed OSIRIS, we have been regularly asked: how does the data get onto our website? Because let's be honest: OSIRIS is actually a research information**management**system. It is mainly used for the intuitive **management** of research information. To present research to the outside world, you need another system.

<!-- more -->

Most institutions use their own websites (usually developed by external companies) for this purpose. There are various ways to get the data from a research information system (CRIS) onto the website. The most commonly used are

- The data is retrieved from the website directly via the CRIS API. The advantage is that the data is always up to date. The disadvantage is that this requires development work, as the website has to be specially adapted to the API. In addition, if the CRIS is expanded, the system may also have to be adapted, which can quickly become costly.
- The data is *mediated* between the CRIS and the website by an additional script. A separate script is written that transfers the data at regular intervals (e.g. using a CRON job). The advantage is that if changes are made to the website and CRIS, only the small script needs to be adapted. The disadvantage is that the data is not always up-to-date, as the transfer usually takes place at fixed times.

Last autumn, I therefore had the idea of developing a platform that would offer a comprehensive solution for the web presentation. A perfectly harmonised research portal, so to speak, which is interwoven with OSIRIS and thus offers the perfect synergy.

And once again, **finding a name** was more difficult than developing the idea. In the end, we decided on OSIRIS Portfolio because the name reflects everything the platform stands for:
1. by definition, a portfolio is a compilation of things that represent the work output of a person or an institution. For example, an artist portfolio, a product portfolio, etc.
2. the root word *Port* stands for interfaces in the context of IT. Portfolio accesses the OSIRIS interfaces in order to receive the relevant data.
3. the root word also contains the letters O and R, which are also the characteristic letters from the OSIRIS logo. This allows us to create a logo that is directly reminiscent of OSIRIS.

![OSIRIS Portfolio](https://osiris-app.de/img/portfolio_logo.svg)

We have developed Portfolio over the past few months in close co-operation with [DSMZ](https://dsmz.de), which wants to use the system as soon as possible to completely revise the "Research" section of its website. Initially, Portfolio, like OSIRIS, was also developed in PHP. However, the limitations of this prototype quickly became clear. That's why I completely rewrote it in a modern JavaScript framework (Vue.js) during my summer holiday, making Portfolio modern, interactive and blazingly fast. It accesses a REST API developed specifically for Portfolio, which can of course also be used for other purposes. I have added a new tab in the OSIRIS documentation called *Portfolio API Docs*, in which all endpoints are described with sample data. This API has the advantage that it only delivers what should really be shown. So you can also benefit from the development if you don't want to use Portfolio yourself.

> I will describe exactly what Portfolio can do in the next few weeks in further blog posts and soon there will also be a link to the Github.


