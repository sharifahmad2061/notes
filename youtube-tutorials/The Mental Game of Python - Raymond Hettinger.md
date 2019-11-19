# Ten New Strategies
## Chunking & Slicing:
> Random module

Just like human brain which has a small short term memory (7+-2). So does computer CPU have small amount of registers.
therefore we must divide the problems into **small chunks using slicing**. eg. Dividing a 5 size problem into 1 size problem
and then further dividing the 1 size problem to 0 by using **Aliasing**. Aliasing means linking existing knowledge to the new problem.
On intel CPUs when we do **move ax, bx**. It just simply renames ax as bx.
When thinking about program statements. we use our **COGNITIVE CAPACITY**.
|                   |                   |
|-------------------|-------------------|
|50 + random() * 200| 3 memory registers|
| range(50,201,1)   | 1 memory register |

### Example
`outcomes = ['win','lost','draw','try-again']`

|                   |                   |
|-------------------|-------------------|
|outcomes[int(random() * len(outcomes))]| 3 memory registers|
|  outcomes[randrange(len(outcomes))]   | 1 memory registers|
| choice(outcomes)                      | 0 memory registers|
| [choice(outcomes) for i in range(10)] |A lot of decryption|
|choice(outcomes,k=10)                  |Almost 0 decryption|

For the first computation we have to do alot of decription compared the second computation.
## Solve a related but Simpler problem
## Incremental Development
> Tree Walker

