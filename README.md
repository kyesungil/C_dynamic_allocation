# 1. C_dynamic_allocation fucntion(malloc, calloc, realloc)
1. __프로그램 수행 중에 저장공간이 필요한 경우, OS에 필요한 메모리 양을 요청하여 할당 받아 사용.__
  
2. __언제 동적 할당을 하는가?__    
  -프로그램 수행 전에 필요한 저장 공간 크기를 알수 없는 경우  
3. __어떻게 동적 할당을 하는가?__  
 -시스템 콜 이용  
 -OS는 heap메모리 공간의 공간을 확보하고 확보한 메모리 공간의 주소값을 return  
	-malloc(), realloc(), calloc()
 ## malloc()
   void* malloc(unsigned int)  
  - malloc()은 void type pointer(void*)를 리턴합니다.
  - 첫번째 주소를 리턴해줍니다.(void type pointer이기 때문에 앞에 형변화를 해줍니다.)
  - 메모리 할당에 실해할 경우 NULL을 반환합니다.
  - free() 메모리를 회수합니다.
		- ptr 변수는 stack에 저장되기 때문에 scope를 나가면 pointer는 사라지지만,
		  동적할당 메모리는 사라지지 않는다.
		  동적할당 이후 꼭 free()로 메모리를 회수하자.
		  
    _void*(보이드 포인터)란? 특정 데이터 타입의 포인터로 지정되지 않은 포인터, 사용 전에 데이터 형태를 지정해야 함_    
## calloc()  
void* calloc (size_t num, size_t size)    
- malloc은 할당만하고 초기화 해주지 않는다. 그러나 calloc은 초기화를 해준다.  
## realloc()
void* realloc(void*, unsigned int);

- 이미 할당 받은 받은 메모리 영역을 재조정

- 이전에 할당받은 공간의 __시작 주소값__ 을 알려준다, OS가 기존 공간을 
		확장하여 확보가 불가능하면, 새로운 메모리 공간을 확보하고 return. 
		__이 경우 기존 메모리 공간에 저장되어 있던 정보는 copy됨.__
		
		- doesn't initialize the bytes added
		- returns NULL if can't enlarge the memory block
		- If first argument is NULL, it behaves like malloc()
		- if second argument is 0, it fress the memory block.
# 2. 동적 할당 배열
- 배열은 stack에 있기 때문에 블럭에서 나가면 자동으로 메모리를 제거하지만, __heap에 있는 메모리는 프로그래머가 요청하기 전까지 남아있다.__
### 2-1. 1차원 배열을 2차원 배열처럼 사용하기
    row = 3 , col = 2
    (r, c)

    2D
    (0, 0) (0, 1)
    (1, 0) (1, 1)
    (2, 0) (2, 1)

    1D
    (0, 0) (0, 1) (1, 0) (1, 1) (2, 0) (2, 1)
     0      1      2      3      4      5    
     
     = c + col * r  (index)
### 2-2. 1차원 배열을 3차원 배열처럼 사용하기
    row = 3, col = 2, depth = 2

    (0, 0, 0)(0, 1, 0)
    (1, 0, 0)(1, 1, 0)
    (2, 0, 0)(2, 1, 0)
    ------------------
    (0, 0, 1)(0, 1, 1)
    (1, 0, 1)(1, 1, 1)
    (2, 0, 1)(2, 1, 1)

    1D
    (0, 0, 0)(0, 1, 0)(1, 0, 0)(1, 1, 0)(2, 0, 0)(2, 1, 0)(0, 0, 1)(0, 1, 1)(1, 0, 1)(1, 1, 1)(2, 0, 1)(2, 1, 1)
     0        1        2        3        4        5        6        7        8        9        10       11  
     
    = c + col * r + (col * row) * d (index)
### 2-3. 1차원 배열을 4차원 배열처럼 사용하기
    row col depth height
    (r, c, d, h)

    c + col * r + (col * row ) * d + (col * row * depth) * h (index)
