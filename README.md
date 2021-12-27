# Hash-Table
#include<bits/stdc++.h>
using namespace std;

int prime[]={1031,2053,4099,8209,16411,32771,65537,131101,262147,524309,1048583,2097169,4194319,8388617,16777259,33554467,67108879,134217757,268435459,536870923,1073741827};

class Hash
{
	public:
		Hash(int find_prime_location);  // Constructor

		// inserts a key into hash table
		void insertItem(int x);

		// search a key from hash table
		void searchItem(int x);

		// hash function to map values to key
		int hashFunction(int x) {
			int array[4];
			for(int i=0;i<4;++i){
				array[i] = x % 256;
				x >>= 8;
			}
			int sum=0;
			sum = (r1*array[0])+(r2*array[1])+(r3*array[2])+(r4*array[3]);
			return sum % M;
		}
		void random_r();

	private:
		// Pointer to an array containing buckets
		list<int> *table;

		int r1;
		int r2;
		int r3;
		int r4;
		int M;
};

Hash::Hash(int location)
{
	random_r();
	M = prime[location];
	table = new list<int>[M];
}

void Hash::insertItem(int key)
{
	int index = hashFunction(key);
	table[index].push_back(key);
}

void Hash::searchItem(int key)
{
	int index = hashFunction(key);
	list<int>::iterator it;
	it = find(table[index].begin(), table[index].end(),key);
}

void Hash::random_r()
{
	r1 = rand()%262144+1; //2^18
	r2 = rand()%262144+1;
	r3 = rand()%262144+1;
	r4 = rand()%262144+1;
}

int main()
{
	time_t t;

	srand((unsigned)time(&t));

	for(int j=10;j<31;++j){
		long long int total = pow(2,j);
		int *input = new int[total];
		int capacity = sizeof(input) / sizeof(input[0]);
		int *array = new int[total];
		int n=0;

		int find_prime_location = j-10;

		for(int i=0;i<total;++i){
			input[i] = rand()%1073741824+1;
		}
		clock_t start,end;
		start = clock();
		Hash h(find_prime_location);
		for(int q=0;q<total;++q){
			h.insertItem(input[q]);
		}		
		end = clock();
		double result = (double)(end - start) / (CLOCKS_PER_SEC);
		cout << "Hash Table insert: 2^" << j << ", " << result << endl;
		delete[] input;

		/*********************************************************************/

		int *search = new int[100000];
		capacity = sizeof(search) / sizeof(search[0]);
		for(int i=0;i<100000;++i){
			search[i] = rand()%1073741824+1;
		}
		start = clock();
		for(int q=0;q<100000;++q){		
			h.searchItem(search[q]);
		}
		end = clock();
		result = (double)(end - start) / (CLOCKS_PER_SEC);
		cout << "Hash Table search: 2^" << j << ", " << result << endl << endl;
		delete[] search;
	}
	return 0;
}
