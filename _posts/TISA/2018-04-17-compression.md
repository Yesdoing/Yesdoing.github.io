---
layout: post
title: "압축(Compression)"
categories:
  - 알고리즘
tags:
---

![사진](https://d81pi4yofp37g.cloudfront.net/wp-content/uploads/algorith.png)
#### 이 포스팅의 알고리즘 강의는 [이곳](https://www.inflearn.com/course/%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-%EA%B0%95%EC%A2%8C/)에서 보실 수 있습니다.

### Huffman Coding
- 가령 6개의 문자 a, b, c, d, e, f로 이루어진 파일이 있다고 하자. 문자의 총 개수는 100,000개이고 각 문자의 등장 횟수는 다음과 같다.
- Frequency는 문자의 등장횟수, Fixed-length code 는 고정길이 표현방법(3비트), Variable-length는 가변길이 표현방법이다.(표현방법에 중복된 표현은 존재하면 안된다.)

|      | a     | b     | c     | d     | e     | f     |
| :------------- | :------------- | :------------- | :------------- | :------------- | :------------- | :------------- |
| Frequency       | 45       | 13       | 12       | 16      | 9       | 5       |        |
| Fixed-length code       | 000       | 110       | 010       | 011       | 100       | 101       |
| Variable-length       | 0       | 101       | 100       | 111       | 1101       | 1100       |

- 고정길이 코드를 사용하면 각각의 문자를 표현하기 위해서 3비트가 필요하며, 따라서 파일의 길이는 300,000비트가 된다.
- 위 테이블의 가변길이 코드를 사용하면 224,000비트가 된다. => 압축효과

#### Prefix Code
- 어떤 codeword도 다른 codeword의 prefix가 되지 않는 코드 (여기서 codeword란 하나의 문자에 부여된 이진코드를 뜻한다.)
- 모호함이 없는 decode가 가능함
- prefix code는 하나의 이진트리로 표현 가능함 (문자가 leaf node에 위치한다.)
![사진](https://drive.google.com/uc?id=1-TCgT1BZs89RL8b6hAobX4PYERgF7_WF)
- Prefix code를 사용해서 압축을 할 시 Huffam coding 보다 효율이 좋을 수 없다.
![사진](https://drive.google.com/uc?id=17kdzW7qtIkfJG8SAkRi19qkS2aHNE08_)
![사진](https://drive.google.com/uc?id=16BMaKt0mPLAndP5Xx940T3p2ooepDF5q)
![사진](https://drive.google.com/uc?id=1NR4phSuPwYZDvY-V6TeIkUV19B-Wfd9B)
1. 제일 처음에는 싱글 노드 트리가 5개가 있다고 생각한다.({}, {}, {}, {}, {})
2. 그 중 가장 작은 것(트리로 만들 값을 선택해서) 2개를 골라 트리로 만든다.
3. 계속 해서 반복해서 트리가 완성되면 prefix code로 활용한다.

#### Run-Length Encoding
- 런(run)은 동일한 문자가 하나 혹은 그 이상 연속해서 나오는 것을 의미한다. 예를 들어 스트링 s = "aaabba"는 다음과 같은 3개의 run으로 구성된다: "aaa", "bb", "a".
- run-length encoding에서는 각각의 run을 그 "run을 구성하는 문자"와 "run의 길이"의 순서 쌍 (n, ch)로 encoding한다. 여기서 ch가 문자이고 n은 길이이다. => (3a2b1a)
- Run-length encoding은 길이가 긴 run들이 많은 경우에 효과적이다.

#### Huffman method with Run-length encoding
![사진](https://drive.google.com/uc?id=1ahqI7dmOk7e3PST1gEbOZzTlzmByIHvS)
![사진](https://drive.google.com/uc?id=15vRtkRYOKdGJq2Pkt4kzD4T6lJdY25Zw)


### 제1단계 : Run과 frequency 찾기
- 압축할 파일을 처음부터 끝까지 읽어서 파일을 구성하는 run들과 각 run들의 등장횟수를 구한다.
- 데이터 파일을 적어도 2번 읽어야 한다.
    - Run을 찾아서 허프만 트리 생성
    - 실제로 압축
- Run 인식하기
![사진](https://drive.google.com/uc?id=1l5crrQ7diIEvIGrPRkO-1IjelWVDV-5F)

### 제2단계 : Huffman tree
- Huffman coding 알고리즘은 트리들의 집합을 유지하면서
- 매 단계 가장 frequency가 작은 두 트리를 찾아서
- 두 트리를 합친다.
- 이런 연산에 가장 적합한 자료구조는 최소 힙(minimum heap)이다.
- 즉 힙에 저장된 각각의 원소들은 하나의 트리이다. (노드가 아니라.)

![사진](https://drive.google.com/uc?id=1EmkAg1Z8iOKncHUIqM730ddq8GxVm8nw)
![사진](https://drive.google.com/uc?id=1CO4ZIkvnFpYzG1429G6BPqYS_S2vpRST)
![사진](https://drive.google.com/uc?id=16NghL6_QRYOX4HsFaWzwuBbkTQSkm5D1)
![사진](https://drive.google.com/uc?id=114ApncMfrAutEKFzzM7LCZRK7WY9C4nz)
![사진](https://drive.google.com/uc?id=1TK6vFnFezy-4-r2qegqtEze8O1d__iNx)

### 제3단계 : codeword 부여하기
- 허프만 트리에서 완성된 트리에 코드워드를 부여한다.
![사진](https://drive.google.com/uc?id=1pa6IfsaQ5wXFGfNeY8h1Nlr-SOW4Kpmz)

### 제4단계 : codeword 검색하기
- 데이터 파일을 압축하기 위해서는 데이터 파일을 다시 처음부터 읽으면서 run을 하나씩 인식 한 후 해당 run에 부여된 codeword를 검색한다.
- huffman tree 에는 모든 run들이 리프노드에 위치하므로 검색하기 불편하다.
- 검색하기 편리한 구조를 만들어야한다
![사진](https://drive.google.com/uc?id=1uzJddQz3bb8I3zL504o6BLtGDa2WiziZ)

### 제5단계 : 인코딩하기
- 압축파일의 맨 앞부분(header)에 파일을 구성하는 run들에 대한 정보를 기록한다.
- 이때 원본파일의 길이도 함께 기록한다. (각각의 run들에 부여한 코드워드를 알고 있어야 복원가능하기 때문에)

### 제6단계 : 디코딩하기
- 인코딩의 반대로 실행한다.

#### HuffmanCoding
```
package huffman;

import java.io.IOException;
import java.io.RandomAccessFile;
import java.util.ArrayList;
import java.util.Arrays;

public class HuffmanCoding {

	private ArrayList<Run> runs = new ArrayList<Run>();

	private Heap<Run> heap;
	private Run theRoot = null;
	private Run[] chars = new Run[256];
	private long sizeOriginalFile;

	private void outputFrequencies(RandomAccessFile fIn, RandomAccessFile fOut) throws IOException {
		fOut.writeInt(runs.size());
		fOut.writeLong(fIn.getFilePointer());

		for(int j = 0; j < runs.size(); j++) {
			Run r = runs.get(j);
			fOut.write(r.symbol);
			fOut.writeInt(r.runLen);
			fOut.writeInt(r.freq);
		}
	}

	private void createHuffmanTree() {
		Run[] arrRuns = new Run[runs.size()];
		heap = new Heap<Run>(runs.toArray(arrRuns));
		heap.buildMinHeap();

		while(heap.getHeapSize() > 0) {
			Run exMin1 = heap.extractMin();
			Run exMin2 = heap.extractMin();

			Run tree = new Run(exMin1.getFreq() + exMin2.getFreq());
			tree.setSons(exMin1, exMin2);

			heap.minHeapInsert(tree);
		}

		theRoot = heap.extractMin();
	}

	private void printHuffmanTree() {
		preOrderTraverse(theRoot, 0);
	}

	private void storeRunsIntoArray(Run p) {
		if(p.left == null && p.right == null) {
			insertToArray(p);
		} else {
			storeRunsIntoArray(p.left);
			storeRunsIntoArray(p.right);
		}
	}

	private void insertToArray(Run p) {
		if(chars[(int)p.symbol] == null) {
			chars[(int)p.symbol] = new Run(p);
		} else {
			Run temp = chars[(int)p.symbol];
			chars[(int)p.symbol] = new Run(p);
			chars[(int)p.symbol].right = temp;
		}
	}

	public Run findRun(byte symbol, int length) {
		Run temp = chars[(int)symbol];
		while(temp.right != null) {
			if(temp.runLen == length) {
				return temp;
			}
			temp = temp.right;
		}

		return temp;
	}

	private void preOrderTraverse(Run node, int depth) {
		for(int i=0; i<depth; i++)
			System.out.print("    ");
		if(node == null) {
			System.out.println("null");
		} else {
			System.out.println(node.toString());
			preOrderTraverse(node.left, depth+1);
			preOrderTraverse(node.right, depth+1);
		}

	}

	private void collectRuns(RandomAccessFile fIn) throws IOException {

		int ch = fIn.read();
		int runLen = 1;
		byte symbol = (byte)ch;
		while(ch != -1) {
			ch = fIn.read();
			if(symbol != (byte)ch) {
				Run run = new Run(symbol, runLen);

				if(runs.contains(run)) {
					runs.get(runs.indexOf(run)).plusFreq();
				} else {
					runs.add(run);
				}

				symbol = (byte)ch;
				runLen = 1;
			} else {
				runLen++;
			}
		}
	}

	private void assignCodewords(Run p, int codeword, int length) {
		if(p.left == null && p.right == null) {
			p.codeword = codeword;
			p.codewordLen = length;
		} else {
			assignCodewords(p.left, codeword << 1, length+1);
			assignCodewords(p.right, (codeword << 1) + 1, length+1);
		}
	}

	public void compressFile(String inFileName, RandomAccessFile fIn) throws IOException {
		String outFileName = new String(inFileName + ".z");

		RandomAccessFile fOut = new RandomAccessFile(outFileName, "rw");

		collectRuns(fIn);
		outputFrequencies(fIn, fOut);
		createHuffmanTree();
		assignCodewords(theRoot, 0, 0);
		storeRunsIntoArray(theRoot);
		fIn.seek(0);
		encode(fIn, fOut);
	}


	private void encode(RandomAccessFile fIn, RandomAccessFile fOut) throws IOException {
		// TODO Auto-generated method stub
		int buffer = 0;
		int bufferLen = 0;
		int ch = fIn.read();
		int runLen = 1;
		byte symbol = (byte)ch;
		while(ch != -1) {
			ch = fIn.read();
			if(symbol != (byte)ch) {
				Run run = findRun(symbol, runLen);
				System.out.println(run);
				System.out.println("codeword : " + run.codeword + ", codewordLen : " + run.codewordLen);
				bufferLen += run.codewordLen;

				if(bufferLen >= 32) {
					fOut.write(buffer);
					buffer = 0;
					buffer += run.codeword;
					buffer = buffer << run.codewordLen;
					bufferLen = run.codewordLen;
				} else {
					buffer <<= run.codewordLen;
					buffer += run.codeword;					
				}

				symbol = (byte)ch;
				runLen = 1;
			} else {
				runLen++;
			}
		}

		if (buffer != 0) {
			int padding = 32 - bufferLen;
			System.out.println(buffer);
			buffer <<= padding;
			System.out.println(buffer);
			fOut.writeInt(buffer);
		}
	}

	public void decompressFile(String fileName, RandomAccessFile fIn) throws IOException {
		// TODO Auto-generated method stub
		String outFileName = new String(fileName + ".dec");
		RandomAccessFile fOut = new RandomAccessFile(outFileName, "rw");
		inputFrequencies(fIn);
		createHuffmanTree();
		assignCodewords(theRoot, 0, 0);
		printHuffmanTree();
		decode(fIn, fOut);
	}

	private void decode(RandomAccessFile fIn, RandomAccessFile fOut) throws IOException {
		// TODO Auto-generated method stub
		int nbrBytesRead = 0, j, ch, bitCnt = 1, mask = 1, bits = 8;
		mask <<= bits - 1;

		for (ch=fIn.read(); ch != -1 && nbrBytesRead<sizeOriginalFile;) {
			Run p = theRoot;
			System.out.println(ch);
			while(true) {
				if (p.left == null && p.right == null) {
					for(j=0; j<p.runLen; j++) {
						fOut.write(p.symbol);
					}
					nbrBytesRead += p.runLen;
					break;
				}
				else if ((ch & mask) == 0) {
					p = p.left;
				} else p = p.right;

				if(bitCnt++ == bits) {
					ch = fIn.read();
					bitCnt = 1;
				} else ch <<= 1;
			}
		}
	}

	private void inputFrequencies(RandomAccessFile fIn) throws IOException {
		// TODO Auto-generated method stub
		int dataIndex = fIn.readInt();
		sizeOriginalFile = fIn.readLong();
		runs = new ArrayList<>();
		for(int j=0; j < dataIndex; j++) {
			Run r = new Run();
			r.symbol = (byte) fIn.read();
			r.runLen = fIn.readInt();
			r.freq = fIn.readInt();
			System.out.println(r);
			runs.add(r);
		}
	}
}

```

### Huffman Encoder
```
package huffman;

import java.io.IOException;
import java.io.RandomAccessFile;
import java.util.Scanner;

public class HuffmanEncoder {
	public static void main(String[] args) {
		String fileName = "";
		HuffmanCoding app = new HuffmanCoding();
		RandomAccessFile fIn;
		Scanner kb = new Scanner(System.in);
		try {
			System.out.print("Enter a file name: "); ///Users/yesdoing/CodeSquard/workspace/TISA/src/huffman/sample.txt
			fileName = kb.next();
			fIn = new RandomAccessFile(fileName, "r");
			app.compressFile(fileName, fIn);
			fIn.close();
		} catch (IOException e) {
			System.out.println("Cannot open " + fileName);
		}
	}
}

```

### Huffman Decoder
```
package huffman;

import java.io.IOException;
import java.io.RandomAccessFile;
import java.util.Scanner;

public class HuffmanDecoder {
	public static void main(String[] args) {
		String fileName = "";
		HuffmanCoding app = new HuffmanCoding();
		RandomAccessFile fIn;
		Scanner kb = new Scanner(System.in);
		try {
			System.out.print("Enter a file name: ");
			fileName = kb.next();
			fIn = new RandomAccessFile(fileName, "r");
			app.decompressFile(fileName, fIn);
			fIn.close();
		} catch (IOException io) {
			System.err.println("Cannot open " + fileName);
		}
	}
}

```
