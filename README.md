# Database-Systems-CSCI-4380-01-Homework-9-solution

Download Here: [Database Systems, CSCI 4380-01 Homework # 9 solution](https://jarviscodinghub.com/assignment/database-systems-csci-4380-01-homework-9-solution/)

For Custom/Original Work email jarviscodinghub@gmail.com/whatsapp +1(541)423-7793

Question 1. You are given the following two schedules.
(a) list all conﬂicts and draw the conﬂict graph.
(b) discuss whether the schedule is serializable or not with a one sentence explanation of why. If it is serializable, ﬁnd an equivalent serial schedule.
S1 : r1(X) r1(Y ) w2(Y ) w2(Z) r3(Z) w3(K) r2(K) w2(L) w1(X) S2 : r1(X) w2(X) w1(Y ) r3(Y ) w3(Z) r2(Z) r3(W) w4(W) w2(Z) r4(W)
Question 2. You are given the following contents of the log and data pages after a crash. Based on this, discuss which transactions must be aborted, which actions must be undone and which actions must be redone. You can choose to REDO all actions or only those actions by committed transactions. However, you must also REDO all CLR actions as well. You do not need to worry about which new log items are written for the recovery.
First list which transactions need to be aborted. Then, go through each potential redo/undo operation in the order attempted for recovery (forward for REDO and backwards for UNDO), read the data page into memory, discuss what is the last stored change for that page and whether it needs to be redone or undone.
Each update operation is listed as PAGEID UPDATE OLDVALUE NEWVALUE.
For each transaction, the LSN (log sequence number) of the previous oepration for that transaction is given.
1
LOG CONTENTS LSN Xact Action PrevLSN 1 T1 P1 update A B 2 T2 P3 update 1 2 3 T3 P4 update 4 8 4 T1 P2 update DD FF 1 5 T3 P4 update 8 12 3 6 T4 P5 update X1 X3 7 T1 commit 4 8 T5 P1 update B C 9 T3 commit 5 10 T2 P2 update FF HH 2 11 T4 abort 6 12 T4 CLR: undo 6 13 T5 P5 update X1 Y 8 14 T5 P4 update 12 20 13
DATA PAGE CONTENTS Page Content LSN of last recorded change P1 B 1 P2 HH 10 P3 1 0 P4 8 3 P5 X1 12
Question 3. You are given the queries provided answers to Hw#5 and the jeopardy database from Hw #6 already created for you in your personal DB in the class server. You must ﬁnd one index that reduces the cost of one of the queries in Hw#5. Document the improvement as described below. (Note that Query 9 is not applicable because the hw#6 dataset did not include the states table.)
To optimize a query, ﬁrst you will look at its estimated cost before you create an index. To do this, you can simply add the word explain to the beginning of the query. For example:
jeopardy=> explain select fullname from contestants where shortname=’Gilbert’; QUERY PLAN ————————————————————————————–Index Scan using contestants_pkey on contestants (cost=0.29..755.07 rows=4 width=13) Index Cond: ((shortname)::text = ‘Gilbert’::text) (2 rows)
jeopardy=> explain select fullname from contestants where description like ‘%5-day%’; QUERY PLAN ————————————————————-Seq Scan on contestants (cost=0.00..953.76 rows=2 width=13) Filter: ((description)::text ~~ ‘%5-day%’::text) (2 rows)
jeopardy=> explain select fullname from contestants where description like ‘%5-day%’ order by fullname; QUERY PLAN ——————————————————————-Sort (cost=953.77..953.78 rows=2 width=13) Sort Key: fullname -> Seq Scan on contestants (cost=0.00..953.76 rows=2 width=13) Filter: ((description)::text ~~ ‘%5-day%’::text) (4 rows)
In each query plan, you get two estimated costs (e.g. 0.29 and 755.07). The ﬁrst one is the time to the ﬁrst answer (which is non-zero given the unavoidable cost of scanning the index ﬁrst) and
2
the second one is the cost of getting all the answers. In the second query, the initial cost is zero because you can start to produce tuples as soon as you start scanning the relation (assuming you ﬁnd some matching tuples). You can see that the cost of the third query is the same for both ﬁrst and all results because sort is a blocking query, you cannot return any results until the sort is complete. Then, you can return all the results.
To document your answer, simply show the query plan before creating the index, index creation command and the query plan after creating the index. Then, write down your savings, by computing OLD-COST-NEW-COST. This is the improvement we will look at.
Even if your answer was faster than mine, please use of one my answers and simply ﬁnd one index that improves the running time and is used in the query plan.
Who will have the best improvement? Technically we only need one. But if you want to show us more than one, feel free.
Hint. Find the most costly queries. Do not run them, simply run EXPLAIN. These are the ones you can make a diﬀerence on. Look at conditions that have an equality and which attributes are involved in these.
