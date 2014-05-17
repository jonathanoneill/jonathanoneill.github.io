---
layout: post
title:  "Reserve RDS Instance Using AWS CLI"
date:   2010-11-30 00:00:00
categories: [AWS]
---

<p>If you have recently reserved a <a title="Amazon Web Services" href="http://aws.amazon.com/" target="_blank">AWS</a> EC2 instances you will know that it can be done easily in the AWS Management Console by selection the "Purchase Reserved Instances" option.</p>
<p>Then you read that you can now reserve a Amazon Relational Database Service (RDS) instance so you look for the button on the Console and find that Amazon haven't added this feature yet. So what do you do? Here is a step by step guide for novice user of the Command Line Interface (CLI) on how to reserve an instance.</p>
<p><strong>Download public/private keys:</strong></p>
<p>1. Login to http://aws.amazon.com</p>
<p>2. Point to "Your Account" and click "Security Credentials"</p>
<p>3. In the Access Credentials section of the page, click the X.509 Certificates tab.</p>
<p>4. Click Create a New Certificate.</p>
<p>Your X.509 certificate and corresponding private key are generated.</p>
<p>5. From the dialog box, download your private key file and X.509 certificate file.</p>
<p><strong>Setup the RDS CLI:</strong></p>
<p>1. Download the RDS Command Line Interface (CLI) RDSCli.zip</p>
<p>2. Unzip to C:\RDSCli</p>
<p>3. C:\set AWS_RDS_HOME=C:\RDSCli\RDSCli-1.3.003</p>
<p>4. C:\set EC2_CERT=c:\keys\cert-********************************************.pem</p>
<p>5. C:\set EC2_PRIVATE_KEY=c:\keys\pk-********************************************.pem</p>
<p><strong>Reserve Instance:</strong></p>
<p>1. First use the <strong><em>rds-describe-reserved-db-instances</em></strong> command to returns the list of DB Instance reservations for your account or details for one of your reserved database instances.</p>
<p style="padding-left:30px;">C:\rds-describe-reserved-db-instances --region eu-west-1</p>
<p style="padding-left:30px;">If you have no existing reserved RDS instances for the specified region then this will return nothing.</p>
<p>2. Next use <strong><em>rds-describe-reserved-db-instances-offerings</em></strong> to returns the list of DB Instance offerings that are available for purchase.</p>
<p style="padding-left:30px;">C:\rds-describe-reserved-db-instances-offerings --region eu-west-1</p>
<p style="padding-left:30px;font-size:80%;">OFFERING  248e7b75-3883-4c10-b628-5ce44f4b53ea  db.m1.xlarge   n  1y  $1820.00   $0.471  mysql<br />
OFFERING  3a98bf7d-fed8-4912-8e40-f0bb5491d217  db.m1.large    y  1y  $1820.00   $0.471  mysql<br />
OFFERING  4b2293b4-65c7-46c2-a92e-a6c03000a28b  db.m2.4xlarge  y  3y  $16000.00  $2.816  mysql<br />
OFFERING  4b2293b4-679e-4c98-b2e6-35ab9d841f72  db.m2.xlarge   n  1y  $1325.00   $0.352  mysql<br />
OFFERING  60dcfab3-4eeb-4f93-8cfe-998181354391  db.m2.2xlarge  y  3y  $8000.00   $1.408  mysql<br />
OFFERING  60dcfab3-eaf2-4d60-a422-f05ced59f8d1  db.m2.4xlarge  y  1y  $10600.00  $2.816  mysql<br />
OFFERING  649fd0c8-3b34-4876-b3ec-31bf4067bffb  db.m1.xlarge   n  3y  $2800.00   $0.471  mysql<br />
OFFERING  649fd0c8-5c8a-4eb7-b34d-50ebf9cf99cc  db.m1.large    n  3y  $1400.00   $0.235  mysql<br />
OFFERING  649fd0c8-6b22-4d6a-ba19-9c44a434749c  db.m1.small    y  3y  $700.00    $0.118  mysql<br />
OFFERING  649fd0c8-8e0e-4b61-be86-56b61cfed4df  db.m2.4xlarge  n  3y  $8000.00   $1.408  mysql<br />
OFFERING  649fd0c8-c5dc-4ac9-b3f6-4da80d2fbc35  db.m1.large    y  3y  $2800.00   $0.471  mysql<br />
OFFERING  649fd0c8-f8f2-4403-a78c-4d40891d72b1  db.m1.small    n  3y  $350.00    $0.059  mysql<br />
OFFERING  c48ab04c-3dc4-45e4-8fb5-ed524e0d427b  db.m2.xlarge   y  3y  $4000.00   $0.704  mysql<br />
OFFERING  ceb6a579-3643-46fc-bb5c-dfeda95895b0  db.m2.xlarge   y  1y  $2650.00   $0.704  mysql<br />
OFFERING  ceb6a579-4495-45fb-b4ad-752e93c31a27  db.m2.4xlarge  n  1y  $5300.00   $1.408  mysql<br />
<strong>OFFERING  ceb6a579-a4f3-4b55-ac53-7208e3c4b0fd  db.m1.small    n  1y  $227.50    $0.059  mysql</strong><br />
OFFERING  ceb6a579-afd6-4446-bd79-f6c3a6af17fc  db.m2.2xlarge  n  1y  $2650.00   $0.704  mysql<br />
OFFERING  ceb6a579-c113-4a40-be7c-59c4ac3681fa  db.m2.xlarge   n  3y  $2000.00   $0.352  mysql<br />
OFFERING  d586503b-4abb-49fa-837a-415b8e7360e9  db.m1.xlarge   y  3y  $5600.00   $0.942  mysql<br />
OFFERING  d586503b-6fb4-4c87-a1cf-df1535f75d54  db.m1.large    n  1y  $910.00    $0.235  mysql<br />
OFFERING  d586503b-9a28-4512-91b4-173d0a808e69  db.m2.2xlarge  n  3y  $4000.00   $0.704  mysql<br />
OFFERING  e5a2ff3b-511c-4f8e-9737-8899eff42b8d  db.m1.small    y  1y  $455.00    $0.118  mysql<br />
OFFERING  e5a2ff3b-62dc-457a-8320-906818958672  db.m2.2xlarge  y  1y  $5300.00   $1.408  mysql<br />
OFFERING  e5a2ff3b-eb93-4c31-83b1-4db9c473c736  db.m1.xlarge   y  1y  $3640.00   $0.942  mysql</p>
<p>3. Looking at the list of instance types and prices select the ID of the one you require, in my case the <strong> db.m1.small</strong> for 1 year and use the <strong><em>rds-purchase-reserved-db-instances-offering</em></strong> command to purchase it.</p>
<p style="padding-left:30px;">C:\RDSCli\RDSCli-1.3.003\bin&gt;rds-purchase-reserved-db-instances-offering <strong>ceb6a579-a4f3-4b55-ac53-7208e3c4b0fd</strong> --region eu-west-1 -c 1</p>
<p style="padding-left:30px;">RESERVATION  ri-2010-11-17-13-17-40-410  db.m1.small  n  2010-11-17T13:17:40.410Z  1y  1  payment-pending  mysql</p>
<p>4. Check a while later to see the status, it should now be <strong>active</strong>.</p>
<p style="padding-left:30px;">C:\RDSCli\RDSCli-1.3.003\bin&gt;rds-describe-reserved-db-instances --region eu-west-1</p>
<p style="padding-left:30px;">RESERVATION  ri-2010-11-17-13-17-40-410  db.m1.small  n  2010-11-17T13:19:19.272Z  1y  1  active  mysql</p>
<p>All done, you have now purchased your RDS reserved instance.</p>
