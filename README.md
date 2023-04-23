# CircularLinkedList-MusicPlayerWinamp

import java.util.Scanner;
public class Solution {

        public static void main(String[] args) {
            Scanner inputUser = new Scanner(System.in);
            myMusic.winAmp<String> winAmpPlaylist = new myMusic.winAmp<>();

            while (true) {
                String input = inputUser.nextLine();
                String[] command = input.split(" ");
                if (command[0].equals("INSERT")) {
                    winAmpPlaylist.addLast(command[1]);
                }
                else if(command[0].equals("REMOVE")) {
                    winAmpPlaylist.remove(Integer.parseInt(command[1]), false);
                }
                else if(command[0].equals("MOVE")) {
                    winAmpPlaylist.pindah(Integer.parseInt(command[1]), Integer.parseInt(command[2]));
                }
                else if(command[0].equals("PLAYAT")) {
                    System.out.printf(String.format("Sedang diputar: %s\n", winAmpPlaylist.playAt(Integer.parseInt(command[1]))));
                }
                else if(command[0].equals("NEXT")) {
                    System.out.printf(String.format("Sedang diputar: %s\n", winAmpPlaylist.pindahNext()));
                }
                else if(command[0].equals("PREV")) {
                    System.out.printf(String.format("Sedang diputar: %s\n", winAmpPlaylist.pindahBefore()));
                }
                else if(command[0].equals("EXIT")) {
                    break;
                }
            }
        }
    }
    class myMusic {
        static class winAmp <M>{
            int size = 0;Node head, tail,now;
            private class Node{
                Node prev,next;
                M nilai;
                Node(){}
                Node (M nilai){this.nilai = nilai;}}

            void addLast(M nilai){
                Node node = new Node(nilai);
                if(head==null){
                    head=tail=node;
                } else {
                    tail.next = node;
                    node.prev = tail;
                    tail=this.tail.next;
                }
                head.prev=tail;
                tail.next=head;
                size++;
            }
            void remove(int idx, boolean berpindah){
                if (head == null){
                    return;
                }
                M musik = null;
                if(now != null){
                    musik = now.nilai;
                }
                idx = Indexget(idx);
                if(idx == 0 ){
                    if (head.next == head){
                        size = 0;
                        now = tail = head =null;
                        return;
                    }if (!berpindah && now != null){
                        if(musik.equals(head.nilai)){
                            now = now.next;
                        }
                    }
                    tail.next = tail.next.next;head = head.next;head.prev = tail;size--;
                    return;
                }
                if (idx == size -1){
                    if(!berpindah && now !=null){
                        if(musik.equals(tail.nilai)){
                            now =now.next;
                        }
                    }tail = tail.prev;
                    tail.next = head;
                    size--;
                    return;
                }
                //mencari posisi node yang ingin dihapus
                Node currentNode;
                currentNode = head;
                for (int i = 0; i < idx-1 ; i++) {
                    currentNode = currentNode.next;
                }
                if(!berpindah && now!=null) {
                    if (musik.equals(currentNode.next.nilai)) {
                        currentNode = currentNode.next;
                    }
                }
                currentNode.next = currentNode.next.next;
                currentNode.next.prev = currentNode;
                size--;
            }
            private int Indexget (int idx){
                if (idx >= 0 && idx < size){
                    return idx;
                } else if (idx >= size) {
                    return idx % size;
                } else {
                    return size + (idx%size);
                }
            }
            void insertMusic(M nilai, int idx, boolean Musiksama){
                idx = Indexget(idx);
                Node node = new Node(nilai);
                if (Musiksama){
                    now = node;
                }
                if (idx == 0){
                    if (head == null){
                        head = tail = node;
                    } else {
                        node.next = head;
                        head.prev=node;
                        head=node;
                    }
                    head.prev = tail;
                    tail.next = head;
                    size++;
                } else if(head==null) {
                    return;
                } else if(idx == this.size){
                    addLast(nilai);
                } else {
                    Node cNode;
                    cNode = this.head;
                    for (int i = 0; i < idx-1; i++) {
                        cNode = cNode.next;
                    }
                    node.next = cNode.next;
                    cNode.next.prev = node;
                    cNode.next=node;
                    node.prev = cNode;
                    size++;
                }
            }
            private M Nilaiget (int idx){
                if (head == null){
                    return null;
                } else {
                    Node cNode;
                    cNode = head;
                    for (int i = 0; i < idx; i++) {
                        cNode = cNode.next;
                    }
                    return cNode.nilai;
                }
            }
            public M playAt(int idx) {
                Node currentNode;
                if (head == null) {
                    return null;
                } else {
                    idx = Indexget(idx);
                    currentNode = head;
                    for (int i = 0; i < idx; i++) {
                        currentNode = currentNode.next;
                    }
                    now = currentNode;
                    return currentNode.nilai;
                }
            }
            void pindah(int idx1,int idx2){
                idx1 = Indexget(idx1);
                idx2 = Indexget(idx2);
                boolean Musiksama = false;
                if (idx1 == idx2){
                    return;
                } else if (size == 0){
                    return;
                } else if(now != null){
                    if (now.nilai.equals((Nilaiget(idx1)))) {
                        Musiksama = true;
                    }
                }
                insertMusic(Nilaiget(idx1),idx2,Musiksama);
                if (idx1 <= idx2){
                    remove(idx1,true);
                } else {
                    remove(idx1 + 1,true);
                }
            }

            M pindahNext() {
                if (now == null) {
                    if (head == null) {
                        return null;
                    }
                    now = head;
                } else {
                    now = now.next;
                }
                return now.nilai;
            }

            //        perbedaannya dengan pindah next adalah pada pointer yang ditunjuk now.
            M pindahBefore() {
                if (now == null) {
                    if (head == null) {
                        return null;
                    }
                    now = head;
                } else {
                    now = now.prev;
                }
                return now.nilai;
            }
            //Print perintah-perintah
            public void print(){
                if(head == null){
                    return;
                }Node a = this.head;
                Node b =a;
                for ( b =a; a!=head;){
                    System.out.print(a.nilai + " ");
                    a = a.next;
                }
                System.out.println();
            }
        }
    }



