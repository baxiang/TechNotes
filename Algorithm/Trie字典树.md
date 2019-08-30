```
import java.util.TreeMap;

public class Trie {

    private class Node{
        public boolean isWord;
        public TreeMap<Character,Node> next;

        public Node(boolean isWord) {
            this.isWord = isWord;
            next = new TreeMap<>();
        }
    }
    private Node root;
    private int size;

    public Trie(){
        root = new Node(false);
        size = 0;
    }

    public void add(String word){
        Node currNode = root;
        for(int i = 0;i<word.length();i++){
            char c = word.charAt(i);
            if (currNode.next.get(c) ==null){
                currNode.next.put(c,new Node(false));
            }
            currNode = currNode.next.get(c);
        }
        if (!currNode.isWord){
            currNode.isWord = true;
            size++;
        }
    }



}

```
