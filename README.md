# Starve-free-readers-writers-problem
The given code corresponds to the solution of the classic readers-writers problem in which the two processes possible are read and write where some data value is either read or written upon respectively.
While multiple readers can read from the data files simultaneously, the number of writers who can write simultaneously is 1. Also no read and write operations can undergo simultaneously.
The synchronization is handled using semaphores.
The solution is a starve-free solution in which no process can starve upto infinity. It also follows the First Come First Serve (FCFS) policy.

Variables:
  operations: It is a vector of 'op' which stores all the pending processes which are to be executed. The boolean 'is_read' of every element shows if the                 operation is of the read type or not.
  waiting_writers: It shows the number of write operations which are in the vector but yet to be executed.
  readers: It shows the number of readers currently reading from the data files.
  mutex: It is a semaphore which is used to ensure mutual exclusion of any two processes. Initially, the value is 1.
  write_turnn: This semaphore indicates if the file space is avaible for a write process to be executed. Initially, the value is 1.
  read_turn: This semaphore indicates if the file space is avaible for a read process to be executed. Initially, the value is 1.
  iterator: This is an iterator which iterates over the elements in the 'operations' vector.
  
Intuition: FCFS can be used to ensure that no process is starved till infinity.

Explanation: 
Reader process: If no process is being executed currently or the process is of a 'read' type, the current process will be executed. Else the process will 	be pushed into the vector and until the writer processes before it are all completed. Everytime a reader process is put into execution , the value of 		readers is incremented by 1 and writer_turn is locked so that no process can write. When all of the current reader processes are terminated i.e. readers 	 = 0, the immediate next writer process gets to be processed. We move the iterator to the next position if the reader at the current position is put into 	its critical section.
Writer process: If any process is being execcuted, the writer process needs to wait until the process(es) to get completed. Once all the previous processes 	completed, the writer process gets to enter in its critical section. Whenever a writer process is being executed, it doesn't allow any other process to 	get executed by locking both the write_turn and the read_turn. When the process is terminated, the iterator is incremented. After incremation, if the 	   current process is of the reader's type, read_turn is unlocked and the same for a writer's type process.In case of no more elements, both the semaphores 	are unlocked as in the initial state.
How this solves starve problem? We keep the track of order in which writer and reader process are arriving. We take care that neither any reader nor any writer processes are left behind as both get a chance to execute. The priority is decided on the basis of the order of arriving.
