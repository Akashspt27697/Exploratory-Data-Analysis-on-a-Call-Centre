# Exploratory-Data-Analysis-on-a-Call-Centre
Introduction to Project


Learning Outcomes

Successful completion of this case study will enable the following learning outcomes:

Conversion of complex business problems to analytical problems.

Analytical investigation of financial transactions

Univariate, Bivariate and Multivariate analysis of data

Data storytelling

Problem Background

CallIn PLC is a BPO solutions company based in Poland that handles incoming calls for various banks. They have dedicated teams for each of their clients. This case study is about the team that handles incoming calls for Royal Bank of Scotland (RBS) that is one of the banks that employs service solutions of CallIn PLC. 

RBS Team provides various services:

Information on transactions of checking and saving accounts of existing customers
Computer-generated voice information (through VRU = Voice Response Unit) 
Information for prospective customers
Support for the customers of RBS’s website (internet customers) 
Dataset given along with the case study is for the month of February 1999 for one month’s call flow. 

Data Dictionary

FIELD

FIELD TYPE

LENGTH

EXPLANATION

vru+line

Alpha numeric

6 characters

Each entering phone-call is first routed through a VRU: There are 6 VRUs labelled AA01 to AA06. Each VRU has several lines labelled 1-16. There are a total of 65 lines. Each call is assigned a VRU number and a line number.

Call\_id

Numeric

5 characters

Each entering call is assigned a call id. Although they are different, the id’s are not necessarily consecutive due to being assigned to different VRUs.

Customer\_id

Numeric

Upto 12 characters

This is the identification number of the caller, which identifies the customer uniquely; the ID is zero if the caller is not identified by the system (as is the case for prospective customers, for example).

Priority

Numeric

1 character

The priority is taken from an off-line file. There are two types of customers: (high-)priority and regular:

0 and 1 indicate unidentified customers or regular customers (to be elaborated on below)
2 indicates priority customers
Customers are served in the order of their "Time in Queue".
Priority customers are allocated at the outset of their call 1.5 minutes of waiting-time (in order to advance their position in the queue.) They are also exempt from paying a NIS 7 monthly fee, which regular customers must pay.
Customers have not been told about the existence of priorities.
Until August 1996, all the customers had the same priority - 0. Priorities 1 and 2 were introduced in August 1st, 1996. There still are 0 priority customers, but they are treated as Priority 1. (As we understand it, priority 0 corresponds to those customers that were assigned priority 0 before August 1st and whose priority has not been upgraded.)
Due to a system bug, customer I.D. was not recorded for those who did not wait in queue; hence, their priority is 0.
Type

Text

2 characters

There are 6 different types of services: 

PS - regular activity
PE - regular activity in English
IN - internet consulting
NE -stock exchange activity
NW - potential customer getting information
TT – customers who left a message asking the bank to return their call but, while the system returned their call, the calling-agent became busy hence the customers were put on hold in the queue.
Date

Numeric

5 characters

Year-month-date (YYMMDD)

vru\_entry

Time

6 characters

Time that the phone-call enters the call-center. More specifically, each calling customer must first be identified, which is done by providing the VRU with the customer-id. Hence this is the time the call enters the VRU.

vru\_exit

Time

6 characters

Time of exit from the VRU: either to the queue, or directly to receive service, or to leave the system (abandonment).

Q\_start

Time

6 characters

Time of joining the queue (being put on “hold”). This entry is 00:00:00, for customers who have not reached the queue (abandoned from the VRU).

Q\_exit

Time

6 characters

Time (in seconds) of exiting the queue: either to receive service or due to abandonment.

Outcome

Text

4/5/7 characters

There are 3 possible outcomes for each phone call:

AGENT – service
HANG - hung up
PHANTOM - a virtual call to be ignored (unclear to us – fortunately, there are only few of these.)
Ser\_start

Time

6 characters

Time of beginning of service by agent.

Ser\_exit

Time

6 characters

Time of end of service by agent.

Server

String

Undefined

Name of the agent who served the call. This field is NO\_SERVER, if no service was provided.

 

Problem Introduction

RBS, being large public sector bank, needs to get its accounts audited periodically for public reporting. During audit, in addition to conduct quarterly, half yearly and annual audits, the Board of Directors of the Bank randomly chose the accounts of a certain year to be audited for in-depth assessment of regularities in processes and accounts. This additional activity is done to ensure:

Any past irregularities by the then auditors
Any irregularity that was missed in earlier reporting
To account for any unknown/ unaccounted cash excess and/ or shortfall
Problem Statement

RBS Board has hired services of Brent Associates to audit its accounts and the auditors have sought pay-out details to CallIn PLC. You are working with Brent Associates as data analyst and have been called in the team to render your services and investigate inconsistencies, if any. All information was tallied but the pay-outs given to CallIn PLC for the month of February 1999 were suspiciously incorrect and hence your major task is to audit this information and assess if payout has been accurately done and state exchequer has not been cheated upon. 

As per historical records, the RBS Team at CallIn PLC constituted of following team in February 1999: 

8 agent positions
1 shift-supervisor position
5 agent positions for internet services (in an adjacent room) 
The team operated for:

During weekdays (Sunday to Thursday), the call centre is staffed from 7:00am to midnight. 
During weekends (Friday-Saturday), it closes at 14:00 on Friday and reopens at around 20:00 on Saturday. 
The automated service (VRU) operates 7 days a week, 24 hours a day.
Below is the summary of compensation structure that was contracted between RBS and CallIn PLC in effect during the month of February 1999:

Data Table - link

Fixed Charges: This is the fixed amount a minimum of which will be billed to RBS by CallIn PLC. This is non-negotiable and non-deductible even under the provisions of applied penalties (Refer later part of this section on clarification of penalties applicable on CallIn PLC).
Operation sustenance fees: EUR 100,000/ month (The fixed fee is to be billed to RBS every month from the date of billing)
Monthly third-party usage charges: This amount is charged in lieu of fixed third party vendors on boarded by CallIn PLC on behalf of RBS including but not limited to Cloud Telephone Services, Internet Connections, Network Maintenance and Server Maintenance Fees, Equipment AMC etc. This flat fee is EUR 20,000/ month
Variable Charges:
VRU Call charges: These relate to the usage of VRU facility by RBS customers:
Calls handled by VRU are not billed to RBS upto a duration of 120,000 seconds 
From 120,001st second onwards, VRU calls are charged at EUR 0.5/ second.
Service Time Charges: This is the main avenue where CallIn PLC generates majority of revenue from the bank. This relates to calls that are transferred and handled by agents. 
Upto a total of 30,00,000 seconds, the RBS is billed at EUR 0.5/ second
From 30,00,001st minute onwards, the bill is raised at EUR 1/ second.
Apart from these charges, certain penalties are agreed and contracted which mandates CallIn PLC to be penalized in case of non-satisfactory servicing of agreement upto the standards of RBS bank:

Queuing of customers: RBS does not want its customers to wait in queue for unreasonably long periods. Hence:
Queue time <= 1 minute per call is not penalized 
Queuing time > 1 and <= 3 minutes is penalized at EUR 0.5/ minute. 
Queuing time > 3 and <= 5 minutes is penalized at EUR 1/ minute. 
Any queuing time of > 5 minutes is penalized at EUR 2/ minute
Call Disconnection: Call disconnection penalty is levied over and above the queuing penalty
Every customer that hangs up the call/ disconnects the call due to non-availability of agents is charged at EUR 10/ instance. These can be found by the calls where outcome is “AGENT” and server is “NO-SERVER”. 
If the outcome is “HANG” and server is “NO-SERVER”, the penalty is applied at EUR 5/ instance. These charges are over and above the queue time. 
Please find below the summary of payments done to CallIn PLC by RBS for the services rendered in the month of February 1999:

Verify if the payments are correctly calculated and during the course of case study, answer the given Multiple-Choice Questions.

HINT: To solve the case study, you will have to create 3 columns:

vru\_time: vru\_exit – vru\_entry
q\_time: q\_start-q\_exit
ser\_time: ser\_start-ser\_exit
