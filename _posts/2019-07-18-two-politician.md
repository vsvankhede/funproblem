---
title: Two Politician
layout: post
comments: true
description: How to Download or Use This Theme
keywords: Graph
---

#### Problem
We have built a small social media platform, only for politicians. The platform allows politicians to follow each other. Currently, we are having five politicians on our platform i.e Modi, Obama,  Trump, Rahul Gandhi, and Amit Shah. 

Given a list of pair, where each pair contains follower and following name. For any given two politician A and B, can you find whether A directly or indirectly following B.

#### Example
```
Input:
List of pairs:
{
(Obama, Modi), (Trump, Modi), (Amit Shah, Modi), (Rahul Gandhi, Modi),  
(Trump, Obama), (Modi, Obama), 
(Modi, Trump), 
(Modi, Amit Shah)
}

Find if Modi directly or indirectly following Rahul Gandhi or not. 

Output:
false
```
#### Write Your Code
Copy following code snippet and write your code in `Write your code here` section.
```java
public class TwoPolitician {
    public static class Pair {
        String follower;
        String following;

        public Pair(String follower, String following) {
            this.follower = follower;
            this.following = following;
        }
    }

    public static void main(String... args) {
        String M = "Modi";
        String O = "Obama";
        String A = "Amit Shah";
        String T = "Trump";
        String R = "Rahul Gandhi";

        Pair modiFollower1 = new Pair(O, M);
        Pair modiFollower2 = new Pair(T, M);
        Pair modiFollower3 = new Pair(R, M);
        Pair modiFollower4 = new Pair(A, M);
        Pair trumpFollower1 = new Pair(M, T);
        Pair obamaFollower1 = new Pair(M, O);
        Pair obamaFollower2 = new Pair(T, O);
        Pair amitFollower1 = new Pair(M, A);

        List<Pair> pairs = Arrays.asList(modiFollower1, modiFollower2, modiFollower3, modiFollower4, 
                                    trumpFollower1, 
                                    obamaFollower1,obamaFollower2,
                                    amitFollower1);

        boolean isFollowing = isFollower(pairs, R, A);

        if(isFollowing) {
            System.out.println("Correct following");
        } else {
            System.out.println("Incorrect! not following");
        }
    }

    public static boolean isFollower(List<Pair> pairs, String polA, String polB) {
        // Write your code
    }
}
```


#### Solution
We can solve this problem using graph data structure. Create a graph of politians and their followers. Then using BFS we can find the path between politician A and B.
```java
public class TwoPolitician {
    public static class Pair {
        String follower;
        String following;

        public Pair(String follower, String following) {
            this.follower = follower;
            this.following = following;
        }
    }

    public static void main(String... args) {
        String M = "Modi";
        String O = "Obama";
        String A = "Amit Shah";
        String T = "Trump";
        String R = "Rahul Gandhi";

        Pair modiFollower1 = new Pair(O, M);
        Pair modiFollower2 = new Pair(T, M);
        Pair modiFollower3 = new Pair(R, M);
        Pair modiFollower4 = new Pair(A, M);
        Pair trumpFollower1 = new Pair(M, T);
        Pair obamaFollower1 = new Pair(M, O);
        Pair obamaFollower2 = new Pair(T, O);
        Pair amitFollower1 = new Pair(M, A);

        List<Pair> pairs = Arrays.asList(modiFollower1, modiFollower2, modiFollower3, modiFollower4, 
                                    trumpFollower1, 
                                    obamaFollower1,obamaFollower2,
                                    amitFollower1);

        boolean isFollowing = isFollower(pairs, R, A);

        if(isFollowing) {
            System.out.println("Correct following");
        } else {
            System.out.println("Incorrect! not following");
        }
    }

    public static boolean isFollower(List<Pair> pairs, String polA, String polB) {
        return isFollower(buildGraph(pairs), polA, polB, new HashSet<String>());
    }

    public static boolean isFollower(Map<String, Set<String>> graph, String start, 
                                                    String dest, Set<String> visited) {
        if(start == dest) return true;
        if(visited.contains(start) || graph.get(start) == null) return false;

        visited.add(start);
        for(String flw: graph.get(start)) {
            if(isFollower(graph, flw, dest, visited))
                return true;
        }
        return false;
    }

    public static Map<String, Set<String>> buildGraph(List<Pair> pairs) {
        Map<String, Set<String>> graph = new HashMap<>();

        for(Pair pair: pairs) {
            Set<String> followers = graph.get(pair.following);
            if(followers == null) {
                followers = new HashSet<String>();
                graph.put(pair.following, followers);
            }

            followers.add(pair.follower);
        }
        return graph;
    }
}
```