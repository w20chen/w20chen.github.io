---
layout:     post
title: 		Huffman Coding
subtitle: 	
date:      	2021-07-30
author:    	Chen
header-img: img/post-bg-keybord.jpg
catalog:	true
tags:
    - CS
---
### Keynote
Step1: Acquire the frequency of characters<br>
Step2: Use a heap to build up the Huffman Tree, according to characters' frequencies.<br>
Step3: Use DFS to print the path of each leaf-node.<br>

### Result
```
ASCII code
32 000
114 00100
117 001010
109 001011
121 001100
118 0011010
45 001101100
35 001101101000000
36 001101101000001
92 001101101000010
96 001101101000011
124 001101101000100
37 001101101000101
62 00110110100011
60 001101101001
34 00110110101
61 0011011011
39 001101110
41 001101111
108 00111
116 0100
104 01010
102 010110
119 010111
105 0110
111 0111
97 1000
112 100100
103 100101
107 1001100
57 10011010000
50 10011010001
49 1001101001
59 1001101010
43 1001101011
40 100110110
38 1001101110000
55 1001101110001
56 100110111001
123 10011011101
125 10011011110
54 10011011111
46 100111
110 1010
100 10110
99 10111
115 1100
10 110100
33 1101010000
58 1101010001
122 1101010010
53 11010100110
94 110101001110
126 1101010011110
64 1101010011111
9 11010101
44 1101011
98 110110
91 1101110000
93 1101110001
95 110111001
106 110111010
120 110111011
63 1101111000
51 11011110010
47 11011110011
42 11011110100
52 11011110101
113 1101111011
48 11011111
101 111
```

### Code(C++) 
```cpp
int times[_ASCII_MAX_ + 1];

void get_statistics(void);
void build_tree(void);
void get_path(void);
void free_tree(void);

int main(void) {
	get_statistics();
	build_tree();
	get_path();
	free_tree();
	return 0;
}

void get_statistics(void) {
	FILE* fp = fopen("source.txt", "r");
	if (fp == nullptr) {
		printf("Cannot open source.txt\n");
		exit(EXIT_FAILURE);
	}
	int tol = 0;
	for (char ch = 0; !feof(fp);) {
		if (fscanf(fp, "%c", &ch) == EOF) break;
		ch = tolower(ch);
		if (ch > _ASCII_MAX_ || ch < 0) continue;
		times[ch]++;
		tol++;
	}
	for (int i = 0; i < _ASCII_MAX_; i++) {
		if (times[i] < 1) continue;
		printf("%c	%d	%f\n", i, times[i], (double)times[i] / tol);
	}
	fclose(fp);
}

struct tree_node {
	char c;
	tree_node* lchild;
	tree_node* rchild;
};

struct root_node {
	int times;
	tree_node* next;
} root[_ASCII_MAX_ + 1];



//a heap (based upon a vector)
struct {
	std::vector<root_node> base;
	int num;
} heap;

//small-topped heap

bool cmp(root_node p1, root_node p2) {
	return p1.times > p2.times;
}

void build_heap(void) {
	//push an initialized root_node into a vector and make the vector into a heap
	heap.num = 0;
	for (int i = 0; i <= _ASCII_MAX_; i++) {
		if (times[i] < 1) continue;
		//ignore those characters which didn't show up
		root_node root{};
		root.times = times[i];
		root.next = new tree_node;
		root.next->c = i;
		root.next->lchild = nullptr;
		root.next->rchild = nullptr;
		heap.base.push_back(root);
		heap.num++;
	}
	std::make_heap(heap.base.begin(), heap.base.end(), cmp);
}

void build_tree(void) {	
	build_heap();
	for (;;) {
		if (heap.num <= 1) break; 
		//end the loop when there is only one root (which is the root of Huffman Tree)
		root_node temp1 = heap.base[0];
		std::pop_heap(heap.base.begin(), heap.base.end(), cmp);
		//swap the first and the last root_node and make all nodes except the last into a new heap
		heap.base.pop_back();
		//the last one got deleted
		heap.num--;
		//make the two smallest roots into a new tree
		heap.base[0].times += temp1.times;
		tree_node* temp2 = heap.base[0].next;
		heap.base[0].next = new tree_node;
		heap.base[0].next->c = _INVALID_CHAR_;
		heap.base[0].next->lchild = temp2;
		heap.base[0].next->rchild = temp1.next;
		//rebuild the heap (new tree included)
		std::make_heap(heap.base.begin(), heap.base.end(), cmp);
	}
}

FILE* fp_d = fopen("Huffman_Coding.txt", "w");

std::deque<int> s;
//a stack, actually

void DFS(tree_node* root) {
	if (root->lchild == nullptr) {
		//in a Huffman Tree, if left child isn't null, nor is right child.
		//print the stack
		fprintf(fp_d, "%c\t", root->c);
		for (int i = 0; i < s.size(); i++) {
			fprintf(fp_d, "%d", s.at(i));
		}
		fputc('\n', fp_d);
		s.pop_back();
	}
	else {
		//if *root is not a leaf
		s.push_back(0);
		DFS(root->lchild);
		s.push_back(1);
		DFS(root->rchild);
		//the stack has been already empty when you go back to the major root
		if (!s.empty()) s.pop_back();
	}
}

void get_path(void) {
	//just for robustness
	if (heap.base[0].next == nullptr || heap.base[0].next->lchild == nullptr 
		|| heap.base[0].next->rchild == nullptr) {
		exit(EXIT_FAILURE);
	}
	DFS(heap.base[0].next);
	fclose(fp_d);
}

void del(tree_node* root) {
	if (root == nullptr) return;
	del(root->lchild);
	del(root->rchild);
	delete root;
}

void free_tree(void) {
	del(heap.base[0].next);
}
```
