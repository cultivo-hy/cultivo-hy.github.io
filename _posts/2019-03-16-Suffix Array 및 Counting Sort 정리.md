---
layout: post

title: "[문자열] 접미사배열(Suffix Array)과 LCP(Longest Common Prefix), Counting Sort 정리"

tags:

- suffix array
- python
- lcp
- lcs
- counting sort

category:
- suffix array
- python
- lcp
- lcs
- counting sort

---
* toc
{:toc}

Suffix Array(접미사 배열)은 어떤 문자열의 suffix(접미사)들을 사전순으로 나열한 배열을 의미한다. 특정문자열에서 반복적으로 나타나는 부분문자열을 찾는 문제에 자주 쓰이는 방법이다. 예를들어 문자열 **ATTCTCG**가 있을경우
접미사 배열은 다음과 같이 만들어 진다. 그림을 보면 문자열 앞에서부터 한칸씩 나가면서 문자열을 잘라낸다.
그리고 이렇게 잘라낸 문자열을 정렬하고 난 뒤 LCP(Longest Common Prefix) 를 구하면 된다. LCP[i]의 값은 Suffix Array에서 **i**번째 접미사와 **i-1**번째 접미사의 가장 긴 공통 접두사의 길이이다. 여기서 LCP의 값이 가장 큰 것이 바로 해당 문자열에서 가장 긴 반복 부분문자열의 길이이다.



![1552623515645](https://user-images.githubusercontent.com/23732754/54452355-50efba80-4798-11e9-8c73-758b825cb1ff.png)

일반화하여 문자열 길이가 N일 때, N개의 접미사를 다 생성한 후 정렬할 때 한 번의 비교에 O(N)의 시간이 걸리고 접미사는 N개이므로 일반적인 정렬 반복횟수는 O(NlogN)이 되어, 전체적인 시간복잡도는 다음과 같다.
$$
O(N^2 logN)
$$
위 알고리즘의 경우 N 이 커질 경우 사용할 수 없게 된다.  이를 해결하기 위해 다른 방법을 생각해보자. d=1부터 시작해서, **매번 접미사의 앞 d * 2글자만으로 정렬한다. 매 루프가 지날 때마다 d는 2배가 된다. **즉 d가 1, 2, 4, 8, ... 식으로 늘어나 정렬을 할 경우 O(logN) 번째 걸리는 것을 알 수 있다. 전체 정렬에 O(NlogN)이 걸린다면 전체 시간복잡도는 다음과 같다.
$$
O(N(logN)^2)
$$
ATTCTCG를 길이 1 단위로 그룹을 지어보면 다음과 같다. 사전순으로 String에 그룹숫자를 부여한다.



![1552638307906](https://user-images.githubusercontent.com/23732754/54452408-6ebd1f80-4798-11e9-8b55-4849cb88ab91.png)



이후 길이 d=1 단위로 본다는 것은, string[0..1], string[1..2] , string[2..3], .. 값들을 비교하는 것이다. 아래 그림에서 길이 2 * 1의 string을 비교하여 그룹숫자를 부여하면 다음과 같다.



![1552638328262](https://user-images.githubusercontent.com/23732754/54452424-7bda0e80-4798-11e9-8107-06fc905e0c00.png)



결과에서 보면 청록색 부분의 string, g[3]과 g[5]이 같은 그룹인덱스인 것을 확인할 수 있다. 
**여기서 3 길이 단위를 구하지 않고,  4길이 단위를 바로 구할수 있다. 아래 그림을 보자.** 



![1552638484924](https://user-images.githubusercontent.com/23732754/54452442-87c5d080-4798-11e9-9617-47f3219d054b.png)



위 d=1 에서 구분짓지 못했던 g[3]과 g[5]를 확인하려면 연두색 부분을 보면 된다. 이때 T와 G, C와 *를 순서대로 비교하는 것은 효율적이지 못하다. 우리는 이미 d=1 값을 구하면서 연두색값에 해당하는 그룹인덱스를 구한것이 주황색임을 알수있다. 즉,  t개 길이 단위에서 g값이 결정되어 있을때 아래식을 비교하면 된다.
$$
if (g[x] == g[y]) \rightarrow compare \space g[x+t] \space , \space g[y+t]
$$
그러므로 2개 길이 단위에서 4개길이 단위를 만드는데, 최대 비교회수는 2번이다.  간단히 얘기하자면, 코딩할 때 (g[x],g[x+t])와 (g[y],g[y+t])를 pair<int,int>로 만들어 놓고 정렬하면 된다는 것이다.  같은 방식으로 [4개 단위에 대한 그룹인덱스]를 가지고 있다면 [8개 단위의 새로운 그룹인덱스]를 만들때에도 최대 2번까지만 비교하게 된다.

 Sorting 알고리즘을 Counting sort로 선택할 경우 정렬 반복횟수는 O(N)이 되어, 전체적인 시간복잡도는 다음과 같다.
$$
O(NlogN)
$$


Counting Sort는 위에서든 예시처럼 정렬하는 String이 특정한 범위(숫자범위, 혹은 알파벳)안에 있을 때 유용하게사용합니다.  문자열 ATTCTCG가 있을 때 counting sort는 말 그대로 각 문자의 갯수를 counting 합니다. 그리하여 얻은 counting 배열을 Accumulated 하여 Accumulated Array를 생성합니다.



![1552663505847](https://user-images.githubusercontent.com/23732754/54452466-9a400a00-4798-11e9-88b0-7579e6cd1485.png)



이후 문자열 끝부터 (1) 시작하여 Accumulated Array에 해당하는 값의 인덱스값을 체크합니다(2). 그리고 빈 Array에 해당 값을 복사한 뒤(3) 인덱스값을 한칸 앞으로 당긴 뒤(4), 이와같은 과정을 반복하면 N번만에 정렬이 됩니다. 이렇게 빠른 속도로 정렬이 가능하지만  대부분 상황에서 많은 메모리 낭비하는 단점이 있습니다.



![1552663556322](https://user-images.githubusercontent.com/23732754/54452492-a7f58f80-4798-11e9-8d72-67ffca2193f8.png)



![1552665326435](https://user-images.githubusercontent.com/23732754/54452521-b93e9c00-4798-11e9-8698-7a95c9be3daa.png)

 Counting Sort for stable sorting

```
typedef pair<int,int> pii;
vector<pii> sort(vector<pii> &v) {
    int n=v.size();
    vector<pii> ans(n);
    vector<int> cnt(n+1,0);
    vector<int> idx(n,0);
 
    for(int i=0; i<n; i++) cnt[v[i].second]++;
    for(int i=1; i<=n; i++) cnt[i]+=cnt[i-1];
    for(int i=n-1; i>=0; i--) idx[--cnt[v[i].second]]=i;
 
    cnt.clear(); cnt.resize(n+1,0);
    for(int i=0; i<n; i++) cnt[v[i].first]++;
    for(int i=1; i<=n; i++) cnt[i]+=cnt[i-1];
    for(int i=n-1; i>=0; i--) ans[--cnt[v[idx[i]].first]]=v[idx[i]];
 
    return ans;
}

12: cnt배열을 이용해서 v[i].second의 개수를 세준다.
13: 값을 누적시켜준다.
14: idx값은 second 기준으로 정렬했을 때 값이 아닌 인덱스를 저장한다.
만약 second를 기준으로만 정렬하고 말거라면, ans[--cnt[v[i].second]]=v[i].second;일 것이다.
그런데 우리는 값이 아닌 원래 정렬되기 전 상태의 인덱스가 필요하므로 ans[--cnt[v[i].second]]=i;라고 저장한 것이며,이게 사실 답은 아니니까 ans[]배열이 아닌 idx[]배열에 저장한 것 뿐이다.
그럼 14번째 줄까지 하고 for(int i=0; i<n; i++) printf("%d ", idx[i]);를 실행하게 되면,
second기준으로 정렬했을 때 v[]배열의 인덱스를 순서대로 출력하는 것이다. 즉, 이 순서대로 first에 접근하면 second크기 순으로 first에 접근할 수 있으며, 다시 이전과 같은 방법으로 first를 정렬하면 second순서를 이전과 같이 최대한 유지하면서 first를 정렬할 수 있다.

출처: https://plzrun.tistory.com/entry/Counting-Sort-Radix-Sort [plzrun's algorithm]
```



아래 코드는 두 sequence의 Longest Common Substring를 찾기 위해 Suffix Array와 Longest Common Prefix를 적용한 코드입니다. 파이썬으로 작성되었고,  시간복잡도는 O(Nlog^2N) 입니다.



```
# Find Longest Common Prefix using suffix array
# first, construct 2 suffix array           -   O(Nlog^N + Mlog^M)
# second,  merge 2 suffix array             -   O(N(a) + M(b))     
a , b  = average of Suffix length compared to each other
# second, find LCP                          -

import time
import math

def suffix_array(txt):
    if not txt:
        return []
    txt += chr(0)
    N = len(txt)
    #print "input string = [", txt, "]"

    equivalence = {t: i for i, t in enumerate(sorted(set(txt)))}
    #print "equi=", equivalence                          #bucket number set
    cls = [equivalence[t] for t in txt]
    #print "cls=", cls                                   # first index sorting

    ns = [(2**i)%N for i in range( int( math.ceil( math.log(N,2))))]
    #print "ns=", ns

    for n in ns[:-1]:
        result = sorted(zip(cls, cls[n:]+cls[:n], range(N)))
        result0, result1, inds = list(zip(*result))

        cls[inds[0]] = 0
        for j in range(1, N):
            cls[inds[j]] = cls[inds[j-1]]
            if (result0[j], result1[j]) != (result0[j-1], result1[j-1]):
                cls[inds[j]] += 1
    n = ns[-1]
    result = sorted(zip(cls, cls[n:]+cls[:n], range(N)))
    return list(list(zip(*result))[2][1:])

def merge(sufarr1, sufarr2 , sufArrIndex1 , sufArrIndex2):      # sort(sumLa)  << time check need
    indMerge = []
    i, j = 0, 0
    len1, len2 = len(sufarr1), len(sufarr2)
    while True:
        if i == len1 and j == len2:             break
        if i == len1:           # if finished sufarr1
            indMerge.append(-sufArrIndex2[j]-1)
            j += 1
        elif j == len2:         # if finished sufarr2
            indMerge.append(sufArrIndex1[i]+1)
            i += 1
        else:
            index1, index2 = sufArrIndex1[i], sufArrIndex2[j]
            str1 , str2 = sufarr1[index1:] , sufarr2[index2:]
            if str1 < str2 :              # standard character-by-character comparison rules
                indMerge.append(index1+1)
                i += 1
            elif str1 > str2:
                indMerge.append(-index2-1)
                j += 1
            else:
                indMerge.append(index1+1)
                indMerge.append(-index2-1)
                i += 1
                j += 1
    return indMerge

def getLCP(suffix1, suffix2, indMerge):
    lcp, where = [0] , []
    length = len(indMerge)
    position = 0 if indMerge[0] > 0 else 1
    where.append(position)
    for x in range(length-1):
        index = indMerge[x]
        if indMerge[x] > 0:
            str1 = suffix1[index - 1:]
        else:
            str1 = suffix2[-index-1:]
        index = indMerge[x+1]
        if indMerge[x+1] > 0:
            str2 = suffix1[index - 1:]
            where.append(0)
        else:
            str2 = suffix2[-index-1:]
            where.append(1)
        size = min(len(str1), len(str2))
        count = 0
        for y in range(size):
            if str1[y] == str2[y]:
                count += 1
            else:
                break
        lcp.append(count)
    return lcp, where

def lcped(suffixMerge,seq1,seq2):
    rank , lcp = [] , [0]
    N = len(suffixMerge)
    for i in range(N):
        rank.insert(suffixMerge[i],i)
    length = 0
    for i in range(N):
        k = rank[i]
        if k:
            j = suffixMerge[k-1]
            while True:
                if j == N :
                    break
                if j+length < N / 2:
                    str1 = seq1[j + length]
                elif j + length < N:
                    str1 = seq2[j + length - N / 2]
                else:
                    break
                if i+length < N / 2:
                    str2 = seq1[i + length]
                elif i + length < N:
                        str2 = seq2[i + length - N / 2]
                else:
                    break
                #str1 = seq1[j + length] if j < N / 2 else seq2[j + length - N / 2]
                #str2 = seq1[i + length] if i < N / 2 else seq2[i + length - N / 2]
                if str1 == str2:
                    length+=1
                else:
                    break
            lcp.insert(k,length)
            if length:
                length-=1
        i+=1
    return lcp

def readSequnce(sequence):
    f = open(sequence)
    for line in f:
        seq = line
    f.close()
    return seq

def main(sequence1,sequence2):
    seq1 = readSequnce(sequence1)
    seq2 = readSequnce(sequence2)
    start_time = time.time()
    la1= suffix_array(seq1)
    la2= suffix_array(seq2)
    print len(la1) , len(la2)
    suffixMerge = merge(seq1,seq2,la1,la2)
    lcp, position = getLCP(seq1,seq2,suffixMerge)
    maxValue, maxStr, compare = lcp[0],'',position[0]
    N = len(suffixMerge)
    for i in range(N):
        if i==0: continue
        if lcp[i] > maxValue and position[i] != position[i-1]:
            maxValue = lcp[i]
            maxStr = seq1[suffixMerge[i]-1:suffixMerge[i]+maxValue-1] if suffixMerge[i] > 0 else seq2[-suffixMerge[i]-1: -suffixMerge[i] + maxValue-1]
    print("--- %s seconds ---" % (time.time() - start_time))
    print maxStr, maxValue

if __name__ == '__main__':
    main('seq819200_1.txt','seq819200_2.txt')

```



![1552532578359](https://user-images.githubusercontent.com/23732754/54452556-ca87a880-4798-11e9-9b5a-a32bdb57c1d4.png)

![1552532591551](https://user-images.githubusercontent.com/23732754/54452608-e25f2c80-4798-11e9-8259-547a12a50e90.png)



|  size   |  0.1M   |  0.2M  |   0.4M   |   0.8M   |   1.6M   |
| :-----: | :-----: | :----: | :------: | :------: | :------: |
| time(s) | 132.77s | 549.5s | 2444.29s | 9689.48s | 31983.4s |
|   LCS   |   26    |   80   |   119    |   177    |   170    |



 input 사이즈 크기에 따른 LCS 길이의 차이를 볼 수 있습니다. 위 LCS 는 실제 sequencing data를 사용하였으며,
아래 blast 툴을 통해 확인 해 볼 수 있습니다.



![그림입니다](https://user-images.githubusercontent.com/23732754/54452632-f440cf80-4798-11e9-98d3-5554490043ce.png)  



#### Reference

https://blog.myungwoo.kr/57
https://bowbowbow.tistory.com/8
https://plzrun.tistory.com/entry/Suffix-Array-ONlogNlgN%EA%B3%BC-ONlogN-%EA%B5%AC%ED%98%84-%EB%B0%8F-%EC%84%A4%EB%AA%85
