---
title: "Leetcode 718 Explained - Answers With Unit Tests"
seoTitle: "leetcode algorithm explained - 718 rabbits in forest - golang"
seoDescription: "leetcode algorithm explained - 718 rabbits in forest. fast and efficient. Best golang answer."
datePublished: Sun Mar 05 2023 14:23:58 GMT+0000 (Coordinated Universal Time)
cuid: clevhhw9u000a09jwezaf02p4
slug: leetcode-718-explained-answers-with-unit-tests
tags: algorithms, coding, array, leetcode, coding-challenge

---

## Intuition
<!-- Describe your first thoughts on how to solve this problem. -->
1. if the rabbits are in the same color AND in the answers array, they should answer the same value otherwise they are inconsistent with each other.
2. if the rabbits are in different colors, the answers could be the same or not.
3. if the rabbits in different colors AND have the same value AND in the answers array, the number of rabbits would be bigger than the value itself.

## Approach
<!-- Describe your approach to solving the problem. -->
1. find the rabbits with the same value. Group them together.
2. for each group, find a minimum matched number of colors then calculate the total number.
3. sum the number together.

## Complexity
- Time complexity: 4 ms
<!-- Add your time complexity here, e.g. $$O(n)$$ -->

- Space complexity: 2.4 MB
<!-- Add your space complexity here, e.g. $$O(n)$$ -->

## Code
```

func numRabbits(answers []int) int {
	// handle null and zero cases
	if answers == nil || len(answers) == 0 {
		return 0
	}
	// sort the answers in order
	sort.Ints(answers)

	// init the final sum
	rcount := 0
	// iterate the answers, find group the values by the same value
	lv := answers[0]
	li := 0
	for i := 1; i < len(answers); i++ {
		cur := answers[i]
		// 	compare the current value and the left value
		if cur != lv {
			//  the current value is a new one, which means the li-th to i-1-th values are the same and need to be processed
			//	add new calculated answer to result count
			rcount += calRabbits(lv, i-li)
			// 	update left value and left index to be current one
			lv = cur
			li = i
		}
	}
	// process the last chunk of value
	rcount += calRabbits(lv, len(answers)-li)
	// return the final value
	return rcount
}

func calRabbits(sameVal int, nSameAnswer int) int {
	// given the number of same color answers and the number of answers
	nGroup := math.Ceil(float64(nSameAnswer) / float64(sameVal+1))

	return int(nGroup) * (sameVal + 1)
}
```

## Unit Tests

```

func Test_numRabbits(t *testing.T) {
	type args struct {
		answers []int
	}
	tests := []struct {
		name string
		args args
		want int
	}{
		{
			"5 rabbits",
			args{
				answers: []int{1, 1, 2},
			},
			5,
		},
		{
			"11 rabbits",
			args{
				answers: []int{10, 10, 10},
			},
			11,
		},
	}
	for _, tt := range tests {
		t.Run(tt.name, func(t *testing.T) {
			if got := numRabbits(tt.args.answers); got != tt.want {
				t.Errorf("numRabbits() = %v, want %v", got, tt.want)
			}
		})
	}
}

func Test_calRabbits(t *testing.T) {
	type args struct {
		sameVal     int
		nSameAnswer int
	}
	tests := []struct {
		name string
		args args
		want int
	}{
		{
			"1 vs 1",
			args{
				sameVal:     1,
				nSameAnswer: 1,
			},
			2,
		},
		{
			"0 vs 1",
			args{
				sameVal:     0,
				nSameAnswer: 1,
			},
			1,
		},
		{
			"2 vs 10",
			args{
				sameVal:     2,
				nSameAnswer: 10,
			},
			12,
		},
		{
			"100 vs 1",
			args{
				sameVal:     100,
				nSameAnswer: 1,
			},
			101,
		},
	}
	for _, tt := range tests {
		t.Run(tt.name, func(t *testing.T) {
			if got := calRabbits(tt.args.sameVal, tt.args.nSameAnswer); got != tt.want {
				t.Errorf("calRabbits() = %v, want %v", got, tt.want)
			}
		})
	}
}
```