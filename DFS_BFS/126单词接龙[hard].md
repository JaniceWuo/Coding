## 题目
给定两个单词（beginWord 和 endWord）和一个字典 wordList，找出所有从 beginWord 到 endWord 的最短转换序列。转换需遵循如下规则：
每次转换只能改变一个字母。
转换过程中的中间单词必须是字典中的单词。
说明:
如果不存在这样的转换序列，返回一个空列表。
所有单词具有相同的长度。
所有单词只由小写字母组成。
字典中不存在重复的单词。
你可以假设 beginWord 和 endWord 是非空的，且二者不相同。

输入:
beginWord = "hit",
endWord = "cog",
wordList = ["hot","dot","dog","lot","log","cog"]

输出:
[
  ["hit","hot","dot","dog","cog"],
  ["hit","hot","lot","log","cog"]
]

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/word-ladder-ii
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

（这题好像在哪场笔试中做过）

## 思考
这题挺难的。因为要输出全部的最短路径，所以除了BFS外还要用到DFS回溯。这里的BFS是来找某个单词的全部邻居（就是改变一个字母得到的wordList中的词）。DFS是当现在遍历到的词是endWord时，还要回溯到上一步进行探索有无其他路径。<br/>
因为要全部可能，所以当遍历完某个词时不能立马将其设为visited，而是遍历完整层再添加。因为如果立马设为visited，就只会输出一种可能。

## code
```java
class Solution {
    public List<List<String>> findLadders(String beginWord, String endWord, List<String> wordList) {
        List<List<String>> ans = new ArrayList<>();
        if(!wordList.contains(endWord)){
            return ans;
        }
        bfs(beginWord, endWord, wordList, ans);
        return ans;
    }

    public void bfs(String beginWord, String endWord, List<String> wordList, List<List<String>> ans) {
        List<String> path = new ArrayList<>();
        Queue<List<String>> queue = new LinkedList<>();
        path.add(beginWord);
        queue.offer(path);
        boolean isFound = false;
        Set<String> dict = new HashSet<>(wordList);
        Set<String> visited = new HashSet<>();
        visited.add(beginWord);
        while(!queue.isEmpty() && !isFound){
            Set<String> subVisited = new HashSet<>();  //每次遍历当前层前进行清空
            int size = queue.size();   //一定要把queue.size单独拿出来保存 因为后面queue的大小会变
            for(int j = 0;j<size;j++){
                List<String> cur_path = queue.poll();
                String temp = cur_path.get(cur_path.size() - 1);  //当前路径的末尾单词
                ArrayList<String> neighbors = getNeighbors(temp, dict);
                for(String neighbor:neighbors){
                    // System.out.println(neighbor);
                    if(!visited.contains(neighbor)){
                        List<String> pathList = new ArrayList<>(cur_path);  //要新建
                        pathList.add(neighbor);
                        if(neighbor.equals(endWord)){
                            isFound = true;
                            ans.add(pathList);
                        }
                        queue.offer(pathList);
                        subVisited.add(neighbor);
                    }
                }
                // System.out.println(subVisited);
            }
            visited.addAll(subVisited);
            
        }
    }


    //找当前单词的全部邻居
    private ArrayList<String> getNeighbors(String cur_word, Set<String> dict){
        ArrayList<String> neighbors = new ArrayList<String>();
        int len = cur_word.length();
        char[] words = cur_word.toCharArray();
        for(char ch = 'a';ch<='z';ch++){
            for(int i = 0;i<len;i++){
                if(ch == words[i]) continue;
                char old_w = words[i];
                words[i] = ch;
                if(dict.contains(String.valueOf(words))){
                    neighbors.add(String.valueOf(words));
                }
                words[i] = old_w;   //又要变回去
            }
        }
        return neighbors;
    }
}
```
