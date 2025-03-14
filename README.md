# Lok-Sabha-Election-Analysis-2024



Project Overview

The "Lok-Sabha-Election-Analysis-2024" project aims to analyze the parliamentary election results of India using SQL Server. It involves querying datasets related to constituency-wise, state-wise, and party-wise election outcomes to extract meaningful insights. The analysis includes examining seat distribution, party alliances, vote margins, and electoral trends across states. The project also classifies parties into alliances such as NDA, I.N.D.I.A., and OTHER to gain a clearer understanding of political dominance

# ERD Diagram
An ERD diagram is included to visually represent the database schema and relationships between tables.

---

![erd2](erd2.png)

# **Database and Tables**


## **Database**
```
create database indian_election;
```

## **Tables**

```
select * from constituencywise_details;

select * from constituencywise_results;

select * from partywise_results;

select * from statewise_results;

select * from states;
```



---

## **Objective**
Total Seats Analysis:

- Determine the total number of parliamentary seats contested in the election.

- State-wise Seat Distribution: Evaluate the number of seats available in each state.

- Party-wise Performance: Analyze how many seats each party and alliance (NDA, I.N.D.I.A., and OTHER) won.

- Voting Trends: Examine the distribution of EVM votes and postal votes per constituency.

- Top Candidate Performance: Identify candidates who received the highest number of votes in EVM ballots
  

## **Identifying the Problems**

With over a billion people, Indian elections involve complex data regarding multiple constituencies, parties, and voter demographics. The challenge lies in efficiently querying large datasets to:

- Understand state-wise and national-level election results.

- Identify party dominance in specific regions.

- Classify parties into alliances for comparative analysis.

- Detect voter preferences based on EVM and postal voting patterns.

---

## **Solving the Problems**
- 1.Total Seats
```
select distinct(count( Parliament_Constituency)) as Total_Seats
from constituencywise_results
```

- 2.What is the total number of seats available for elections in each state
```
select s.state as state_name, count(cr.Parliament_Constituency) from
constituencywise_results cr 
inner join statewise_results sr on cr.Parliament_Constituency=sr.Parliament_Constituency
inner join states s on sr.state_id=s.state_id
group by s.state;
```

- 3.Total Seats Won by NDA Allianz
```
select sum(case when party in  (
'Bharatiya Janata Party - BJP', 
'Telugu Desam - TDP', 
'Janata Dal  (United) - JD(U)',
'Shiv Sena - SHS', 
'AJSU Party - AJSUP', 
'Apna Dal (Soneylal) - ADAL', 
'Asom Gana Parishad - AGP',
'Hindustani Awam Morcha (Secular) - HAMS', 
'Janasena Party - JnP', 
'Janata Dal  (Secular) - JD(S)',
'Lok Janshakti Party(Ram Vilas) - LJPRV', 
'Nationalist Congress Party - NCP',
'Rashtriya Lok Dal - RLD', 
'Sikkim Krantikari Morcha - SKM')
then Won else 0 end) as NDA_Total_Seats_Won
from  partywise_results;
```

- 4.Seats Won by NDA Allianz Parties
```
select party as Party_Name,won as Seats_Won
from  partywise_results where party IN (
'Bharatiya Janata Party - BJP', 
'Telugu Desam - TDP', 
'Janata Dal  (United) - JD(U)',
'Shiv Sena - SHS', 
'AJSU Party - AJSUP', 
'Apna Dal (Soneylal) - ADAL', 
'Asom Gana Parishad - AGP',
'Hindustani Awam Morcha (Secular) - HAMS', 
'Janasena Party - JnP', 
'Janata Dal  (Secular) - JD(S)',
'Lok Janshakti Party(Ram Vilas) - LJPRV', 
'Nationalist Congress Party - NCP',
'Rashtriya Lok Dal - RLD', 
'Sikkim Krantikari Morcha - SKM')
order by Seats_Won desc
```


- 5.Total Seats Won by I.N.D.I.A. Allianz
```
select sum(case  when party in (
'Indian National Congress - INC',
'Aam Aadmi Party - AAAP',
'All India Trinamool Congress - AITC',
'Bharat Adivasi Party - BHRTADVSIP',
'Communist Party of India  (Marxist) - CPI(M)',
'Communist Party of India  (Marxist-Leninist)  (Liberation) - CPI(ML)(L)',
'Communist Party of India - CPI',
'Dravida Munnetra Kazhagam - DMK',
'Indian Union Muslim League - IUML',
'Nat`Jammu & Kashmir National Conference - JKN',
'Jharkhand Mukti Morcha - JMM',
'Jammu & Kashmir National Conference - JKN',
'Kerala Congress - KEC',
'Marumalarchi Dravida Munnetra Kazhagam - MDMK',
'Nationalist Congress Party Sharadchandra Pawar - NCPSP',
'Rashtriya Janata Dal - RJD',
'Rashtriya Loktantrik Party - RLTP',
'Revolutionary Socialist Party - RSP',
'Samajwadi Party - SP',
'Shiv Sena (Uddhav Balasaheb Thackrey) - SHSUBT',
'Viduthalai Chiruthaigal Katchi - VCK'
 ) then Won else 0 end) as INDIA_Total_Seats_Won
from partywise_results
```


- 6.Seats Won by I.N.D.I.A. Allianz Parties
```
select party as Party_Name,won as Seats_Won
from partywise_results where 
party IN (
'Indian National Congress - INC',
'Aam Aadmi Party - AAAP',
'All India Trinamool Congress - AITC',
'Bharat Adivasi Party - BHRTADVSIP',
'Communist Party of India  (Marxist) - CPI(M)',
'Communist Party of India  (Marxist-Leninist)  (Liberation) - CPI(ML)(L)',
'Communist Party of India - CPI',
'Dravida Munnetra Kazhagam - DMK',
'Indian Union Muslim League - IUML',
'Nat`Jammu & Kashmir National Conference - JKN',
'Jharkhand Mukti Morcha - JMM',
'Jammu & Kashmir National Conference - JKN',
'Kerala Congress - KEC',
'Marumalarchi Dravida Munnetra Kazhagam - MDMK',
'Nationalist Congress Party Sharadchandra Pawar - NCPSP',
'Rashtriya Janata Dal - RJD',
'Rashtriya Loktantrik Party - RLTP',
'Revolutionary Socialist Party - RSP',
'Samajwadi Party - SP',
'Shiv Sena (Uddhav Balasaheb Thackrey) - SHSUBT',
'Viduthalai Chiruthaigal Katchi - VCK'
    )
order by seats_won desc
```

- 7.Add new column field in table partywise_results to get the Party Allianz as NDA, I.N.D.I.A and OTHER
```
ALTER TABLE partywise_results
ADD party_alliance VARCHAR(50);


-- for I.N.D.I.A Allianz
update partywise_results
set party_alliance = 'I.N.D.I.A'
where party in (
'Indian National Congress - INC',
'Aam Aadmi Party - AAAP',
'All India Trinamool Congress - AITC',
'Bharat Adivasi Party - BHRTADVSIP',
'Communist Party of India  (Marxist) - CPI(M)',
'Communist Party of India  (Marxist-Leninist)  (Liberation) - CPI(ML)(L)',
'Communist Party of India - CPI',
'Dravida Munnetra Kazhagam - DMK',	
'Indian Union Muslim League - IUML',
'Jammu & Kashmir National Conference - JKN',
'Jharkhand Mukti Morcha - JMM',
'Kerala Congress - KEC',
'Marumalarchi Dravida Munnetra Kazhagam - MDMK',
'Nationalist Congress Party Sharadchandra Pawar - NCPSP',
'Rashtriya Janata Dal - RJD',
'Rashtriya Loktantrik Party - RLTP',
'Revolutionary Socialist Party - RSP',
'Samajwadi Party - SP',
'Shiv Sena (Uddhav Balasaheb Thackrey) - SHSUBT',
'Viduthalai Chiruthaigal Katchi - VCK'
);


-- for NDA Allianz
update partywise_results
set party_alliance = 'NDA'
where party in (
    'Bharatiya Janata Party - BJP',
    'Telugu Desam - TDP',
    'Janata Dal  (United) - JD(U)',
    'Shiv Sena - SHS',
    'AJSU Party - AJSUP',
    'Apna Dal (Soneylal) - ADAL',
    'Asom Gana Parishad - AGP',
    'Hindustani Awam Morcha (Secular) - HAMS',
    'Janasena Party - JnP',
    'Janata Dal  (Secular) - JD(S)',
    'Lok Janshakti Party(Ram Vilas) - LJPRV',
    'Nationalist Congress Party - NCP',
    'Rashtriya Lok Dal - RLD',
    'Sikkim Krantikari Morcha - SKM'
);


-- for OTHER
update partywise_results
set party_alliance = 'OTHER'
where party_alliance is null;


select * from partywise_results;
```


- 8.Which party alliance (NDA, I.N.D.I.A, or OTHER) won the most seats across all states?
```
select pr.party_alliance,count(*) as total_seats from
partywise_results prinner join constituencywise_results cr  
on pr.Party_ID=cr.Party_ID
group by party_alliance order by total_seats desc;
```


- 9.Winning candidate's name, their party name,party_alliance total votes, and the margin of victory for a specific state and constituency?
```
select cr.winning_candidate, pr.party, pr.party_alliance,
cr.total_votes, cr.margin, cr.constituency_name, s.State 
from constituencywise_results cr inner join partywise_results pr on 
cr.party_id=pr.party_id inner join statewise_results sr on 
cr.parliament_constituency=sr.parliament_constituency inner join
states s on sr.state_id=s.state_id 
where cr.constituency_name='KAIRANA';
--where  s.state='Uttar Pradesh'
```


- 10.What is the distribution of EVM votes , postal votes for candidates in a specific constituency?
```
select cd.evm_votes,cd.postal_votes,cd.candidate,
cr.constituency_name from constituencywise_results cr 
inner join constituencywise_details cd on cr.constituency_id=cd.constituency_id
where cr.constituency_name='MUZAFFARNAGAR';
```


- 10.Which parties won the most seats in specific State, and how many seats did each party win?
```
select pr.party,count(cr.constituency_id) as seats_won
from  constituencywise_results cr inner join  
partywise_results pr on cr.party_id = pr.party_id inner join 
statewise_results sr on cr.parliament_constituency = sr.parliament_constituency
inner join  states s on sr.state_ID = s.state_ID
where s.state = 'Uttar Pradesh'
group by pr.Party
order by  seats_won desc;
```


- 11.What is the total number of seats won by each party alliance (NDA, I.N.D.I.A, and OTHER) in each state for the India Elections 2024
```
select s.state,
sum(case when pr.party_alliance ='NDA' then 1 else 0 end) as NDA_Seats_won,
sum(case when pr.party_alliance ='I.N.D.I.A' then 1 else 0 end) as 'I.N.D.I.A_Seats_won',
sum(case when pr.party_alliance ='OTHER' then 1 else 0 end) as OTHER_Seats_won
from constituencywise_results cr inner join  
partywise_results pr on cr.party_id = pr.party_id inner join 
statewise_results sr on cr.parliament_constituency = sr.parliament_constituency
inner join  states s on sr.state_ID = s.state_ID
group by s.state
```


- 12.Which candidate received the highest number of EVM votes in each constituency (Top 10)?
```
select cd.candidate,cd.evm_votes,cr.constituency_name from 
constituencywise_results cr inner join 
constituencywise_details cd on cr.constituency_id=cd.constituency_id
order by evm_votes desc
```


## **Learning Outcomes**

- SQL Querying Skills: Proficiency in writing complex SQL queries to extract, analyze, and manipulate large datasets.

- Data Integration & Joins: Understanding how to join multiple tables effectively to generate meaningful reports.

- Data Aggregation & Classification: Ability to classify data into relevant groups (alliances, parties, constituencies) for analysis.

- Election Trend Analysis: Insights into electoral trends, party dominance, and regional variations in voting patterns.

- Decision-Making & Business Intelligence: Using SQL-driven insights for political strategy, media analysis, and governance planning.

---

## **Conclusion**

The project successfully leverages SQL Server to analyze the Indian Elections 2024 data. By structuring queries to explore constituency-wise, party-wise, and state-wise results, the analysis provides a comprehensive understanding of electoral dynamics. The classification of parties into alliances (NDA, I.N.D.I.A., and OTHER) enables effective comparisons. Additionally, identifying top-performing candidates and understanding voting trends contributes to a deeper insight into India's electoral behavior.  


