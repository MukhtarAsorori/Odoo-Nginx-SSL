# Introduction
Odoo is one the of most popular business softwares in the world and it is packed with multiple useful modules like customer relationship management (CRM), point of sale, project management, inventory management, automated invoicing, accounting, e-commerce, inventory management and much more.

Odoo comes with a built-in web server, but in most cases it is recommended to have a reverse proxy in front of it which will act as an intermediary between the clients and the Odoo server. Once your Odoo is configured and running on the IP with port then proceed with Nginx configuration. 

This guide provides instructions on how to use Nginx as a SSL termination and reverse proxy to Odoo.

# Nginx
A high-performance web server called NGINX was created to meet the growing demands of the modern web. High performance, lots of parallelisms, and sparse resource utilization are its main priorities.

NGINX is a reverse proxy server at its core, despite being primarily recognized as a web server.

But, there are other web servers available besides NGINX. The 1995-first-released Apache HTTP Server (httpd) is one of its main rivals.

Although Apache HTTP Server is more flexible, server administrators frequently favor NGINX for the following two reasons:

It is able to manage more concurrent queries.
It uses fewer resources and delivers static content more quickly.

# Prerequisites
Make sure that you have met the following prerequisites before continuing with this tutorial:

You have Odoo installed, if not you can find the instructions [here](https://github.com/Yenthe666/InstallScript/tree/16.0)
You have a domain name pointing to your Odoo installation.


