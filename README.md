# Mutual-Friend-Analysis-using-Hadoop-MapReduce
This project takes a list of people and their friends list as dataset input.The map stage sorts out the friends of two persons at an instance using the principle of one to many in alphabetical order. In the map output, the two friends are taken as a key and their mutual friends are retrieved as a value pair and sorting is performed.The output is displayed as an adjacency matrix or the list of friends and their mutual friends in key-value pairs. And the project is executed on cloudera. 
The best friendship recommendations often come from friends. The key idea is that if two people
have a lot of mutual friends, but they are not friends, then the system should recommend them to be
connected to each other. Let’s assume that the friendships are undirected: if A is a friend of B then
B is also a friend of A. This is the most common friendship system used in Facebook, Google+,
Linkedin, and several social networks.

###Module-1: Mapper-1

The map script splits data into words. Its output contains the word and 1 as its value. These tuples
become the input of a reducer that sums the word count. The first mapper takes each row and
creates a small string output:

● Split each row into a user number and a string of friends

● Create pairs of the user with each friend with the smaller number first, then append
a 0 to the pair.

● Create combinations of the friends of the user, with the second friend always being
larger than the first friend in the form “friend1, friend2, 1”.


###Module-2: Reducer-1

The Mapper creates pairs of the first letter of each word with a 1. The Reducer counts the number of
words having the same first letter. The first reducer orders and groups keys by value, and lists all
the keys in order by id. The reducer sums values of 1s for each key. If there were no such lines
that had the same key with a 0, then output “friend1 friend2 count of mutuals” for each pair that
was not friends with each other.

<img width="493" alt="Screenshot 2022-02-03 at 12 17 16 AM" src="https://user-images.githubusercontent.com/78052106/152218198-2b544a19-1935-42ab-8d9b-b4e3412ac5ea.png">

###Module-3: Mapper-2

● The second mapper takes this output and slightly changes it:
“f1 tab f2 numMutualFriends” and
“f2 tab f1 numMutualFriends”

● Hadoop streamer orders these outputs by keys and sends them as inputs for the second
reducer

● Input is ordered by user:
(user1 tab friend1, numMutualFriends)
(user1 tab friend2, numMutualFriends) …..
(user 2 tab friend1…)


###Module-4: Reducer-2

● The second reducer picks the top ten potential friends for each user

<img width="501" alt="Screenshot 2022-02-03 at 12 17 32 AM" src="https://user-images.githubusercontent.com/78052106/152218264-5084b5c7-f30d-429b-8521-9d2fa0b1752a.png">

HOW TO RUN THIS ON CLOUDERA
1. Open all files in cloudera.
2. Move hw1input.txt, mapper3a.py,reducer3a.py,mapper3b.py,reducer3b.py,and runscript to cloudera desktop
3. Open cloudera Terminal 
4. Run the following commands 

```

hadoop fs -copyFromLocal /home/cloudera/Desktop/hw1input.txt

```

```

hadoop jar /usr/lib/hadoop-mapreduce/hadoop-streaming.jar -mapper /home/cloudera/Desktop/mapper3a.py -reducer /home/cloudera/Desktop/reducer3a.py -input hw1input.txt -output /user/cloudera/output/*

```

```

hadoop jar /usr/lib/hadoop-mapreduce/hadoop-streaming.jar -mapper /home/cloudera/Desktop/mapper3b.py -reducer /home/cloudera/Desktop/reducer3b.py -input /user/cloudera/output/*/part-00000 -output /user/cloudera/output/**

```

```

hadoop fs -copyToLocal /user/cloudera/output/**

```

and you're all set 
Output file will be in cloudera's Home (/home/**/part-00000)



