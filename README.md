# CircularLinkedList-MusicPlayerWinamp
package Praktikum.tugasMusik;
import java.util.*;
public class CLL {

    public class MusicPlayer {
        static class SongNode {
            String title;
            SongNode prev, next;

            SongNode(String title) {
                this.title = title;
            }
        }

        static class CircularLinkedList {
            SongNode head, tail, pointer;
            int size;

            CircularLinkedList() {
                head = tail = pointer = null;
                size = 0;
            }

            void insert(String title) {
                SongNode newNode = new SongNode(title);
                if (tail == null) {
                    head = tail = newNode;
                    head.prev = tail;
                    tail.next = head;
                } else {
                    tail.next = newNode;
                    newNode.prev = tail;
                    newNode.next = head;
                    head.prev = newNode;
                    tail = newNode;
                }
                size++;
            }

            void move(int fromIndex, int toIndex) {
                if (fromIndex == toIndex) {
                    return;
                }
                if (size == 0) {
                    return;
                }
                if (fromIndex < 0) {
                    fromIndex = fromIndex % size;
                    fromIndex = size + fromIndex;
                }
                if (toIndex < 0) {
                    toIndex = toIndex % size;
                    toIndex = size + toIndex;
                }
                if (fromIndex >= size) {
                    fromIndex = fromIndex % size;
                }
                if (toIndex >= size) {
                    toIndex = toIndex % size;
                }
                SongNode fromNode = head;
                for (int i = 0; i < fromIndex; i++) {
                    fromNode = fromNode.next;
                }
                SongNode toNode = head;
                for (int i = 0; i < toIndex; i++){
                    toNode = toNode.next;
                }
                if(pointer == fromNode){
                    pointer = pointer.next;
                }
                fromNode.prev.next = fromNode.next;
                fromNode.next.prev = fromNode.prev;

                if (toNode == head){
                    head = fromNode;
                }
                fromNode.prev = toNode.prev;
                fromNode.next = toNode;
                toNode.prev.next =  fromNode;
                toNode.prev = fromNode;
            }

            void remove(int index) {
                if (index < 0){
                    index %= size;
                    index += size;
                }

                SongNode curr = head;
                for (int i = 0; i < index; i++) {
                    curr = curr.next;
                }
                if (pointer == curr) {
                    pointer = curr.next;
                }
                if (curr == head) {
                    head = curr.next;
                    head.prev = tail;
                    tail.next = head;
                } else if (curr == tail) {
                    tail = curr.prev;
                    tail.next = head;
                    head.prev = tail;
                } else {
                    curr.prev.next = curr.next;
                    curr.next.prev = curr.prev;
                }
                size--;
            }

            void playAt(int index) {
                if (index < 0){
                    index %= size;
                    index += size;
                }
                SongNode curr = head;
                for (int i = 0; i < index; i++) {
                    curr = curr.next;
                }
                pointer = curr;
                System.out.println("Sedang diputar: " + pointer.title);
            }

            public void playNext() {
                if (pointer == null) {
                    pointer = tail;
                    pointer = pointer.next;
                } else {
                    pointer = pointer.next;
                }
                System.out.println("Sedang diputar: " + pointer.title);
            }

            public void playPrev() {
                if (pointer == null) {
                    pointer = tail;
                    pointer = pointer.next;
                } else {
                    pointer = pointer.prev;
                }
                System.out.println("Sedang diputar: " + pointer.title);
            }

        }
        public static void main(String[] args) {
            CircularLinkedList playlist = new CircularLinkedList();
            Scanner scanner = new Scanner(System.in);

            String command = "";
            while (!command.equals("EXIT")) {
                String[] input = scanner.nextLine().split(" ");
                command = input[0];

                switch (command) {
                    case "INSERT":
                        String title = input[1];
                        playlist.insert(title);
                        break;
                    case "MOVE":
                        int fromIndex = Integer.parseInt(input[1]);
                        int toIndex = Integer.parseInt(input[2]);
                        playlist.move(fromIndex, toIndex);
                        break;
                    case "REMOVE":
                        int indexToRemove = Integer.parseInt(input[1]);
                        playlist.remove(indexToRemove);
                        break;
                    case "PLAYAT":
                        int playIndex = Integer.parseInt(input[1]);
                        playlist.playAt(playIndex);
                        break;
                    case "NEXT":
                        playlist.playNext();
                        break;
                    case "PREV":
                        playlist.playPrev();
                        break;
                    case "EXIT":
                        break;
                    default:
                        System.out.println("Invalid command!");
                        break;
                }
            }

            scanner.close();
        }
    }


}
