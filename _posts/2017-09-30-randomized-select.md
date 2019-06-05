Here’s a question I was asked during my Mattermost job interview:

>Given a million X, Y coordinates how would you find the nearest 100 points?

I didn't have an answer on the spot. The only thing I came up with was a brute-force way of doing it by sorting and then selecting and I suspected that wasn’t optimal (otherwise why ask the question).
                    
Afterwards I did some Googling and apparently it’s an Amazon interview question. Surprisingly I didn’t find any optimal solutions online, so here’s what I did in the days ahead:

I went to my copy of [Introduction to Algorithms](https://mitpress.mit.edu/books/introduction-algorithms) by Cormen (et al), aka CLRS, and found a beautiful divide-and-conquer algorithm called “Randomized-Select” with an expected run time of Θ(n).

Randomized-Select shares some mechanics with Quicksort by partitioning around a pivot. However, because we don’t need both sides of the pivot sorted it has a better expected run time than Quicksort’s Θ(n log n).

The list of X, Y coordinates does have to be iterated first to get distances with √(x² + y²), and from that point on you’re dealing with an array of integers. I wrote it out in Golang:
                    
```go
package selection

import (
	"math/rand"
	"time"
)

// RandomizedSelect is a selection algorithm to select the minimum orderStatistic ints in a slice
func RandomizedSelect(input []int, startIndex int, endIndex int, orderStatistic int) []int {
	if len(input) == 1 {
		return input
	}
	input, pivotIndex := randomizedPartition(input, startIndex, endIndex)
	sortedPartSize := pivotIndex - startIndex + 1
	if orderStatistic == sortedPartSize {
		return input[:pivotIndex+1]
	}
	if orderStatistic < sortedPartSize {
		return RandomizedSelect(input, startIndex, pivotIndex-1, orderStatistic)
	}
	return RandomizedSelect(input, pivotIndex+1, endIndex, orderStatistic-sortedPartSize)
}

func randomizedPartition(input []int, startIndex int, endIndex int) ([]int, int) {
	i := randomInRange(startIndex, endIndex)
	input = swap(input, endIndex, i)
	return partition(input, startIndex, endIndex)
}

func randomInRange(min int, max int) int {
	rand.Seed(time.Now().Unix())
	diff := max - min
	if diff == 0 {
		return 0
	}
	num := rand.Intn(diff) + min
	return num
}

// swap switches the values found at index1 and index2 and returns the new list
func swap(list []int, index1 int, index2 int) []int {
	tempValue := list[index1]
	list[index1] = list[index2]
	list[index2] = tempValue
	return list
}

// Uses the Lomuto partition scheme.
func partition(list []int, left int, right int) ([]int, int) {
	pivot := right
	i := left - 1
	j := left
	for j <= right-1 {
		if list[j] >= list[pivot] {
			j++
		}
		if list[j] < list[pivot] {
			i++
			list = swap(list, i, j)
			j++
		}
	}
	swap(list, i+1, pivot)
	return list, i + 1
}
```

Here’s an example selecting the minimum three from a small list of integers:

```go
package main

import (
	"fmt"
	"github.com/mkraft/selection"
)

func main() {
	input := []int{30, 65, 1, 76, 45, 77, 69}
	result := selection.RandomizedSelect(input, 0, len(input)-1, 3)
	fmt.Println("result", result)
}
```

```bash
$ go run main.go
result [1 30 45]
```

Profiling on my machine with one million inputs results in Randomized-Select being about 32x faster than the Go standard library’s `sort.Ints` for random int16 and int32 keys. Looking at the [source](https://golang.org/src/sort/sort.go?s=9151:9169#L336) for `sort.Ints` it uses various comparison sorting algorithms so I believe the best we would only expect Θ(n log n) from it, which fits with my results.

For keys in a low range (around 100 or less) sorting all one millions integers is actually faster with Counting-Sort. Bucket-Sort may also be faster given certain inputs, but since it was a theoretical exercise there has to be a limit to variations of assumptions: we don’t need to think of all cases.</p>

One of the lessons I learnt from the experience is that with technical interviews you win no matter what: if you don’t know the answer you can go and learn something awesome, and if you do know the answer then you look pretty clever.

Another lesson is that having the answer immediately isn’t the most important quality an engineer can posses. It’s more important to have the aptitude and interest to go out and discover a great solution.