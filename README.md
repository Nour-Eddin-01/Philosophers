# Philosophers

This project is a classic concurrency problem, often referred to as the **Dining Philosophers problem**. The goal is to simulate a scenario where multiple philosophers sit at a circular table, doing three things: eating, thinking, and sleeping. It is designed to learn the basics of threading a process and managing shared memory using mutexes in C.

## Overview

- **Philosophers** sit at a circular table. There is a large bowl of spaghetti in the middle.
- They alternatively **eat**, **think**, or **sleep**.
- While eating, they are not thinking nor sleeping; while thinking, they are not eating nor sleeping; and, of course, while sleeping, they are not eating nor thinking.
- There are as many total forks on the table as there are philosophers.
- A philosopher takes their right and left forks to eat, one in each hand.
- When a philosopher has finished eating, they put their forks back on the table and start sleeping. Once awake, they start thinking again. The simulation stops when a philosopher dies of starvation.
- Every philosopher needs to eat and should never starve. Philosophers don't speak with each other. Philosophers don't know if another philosopher is about to die. No need to say avoiding deadlocks and data races is critical.

## Compilation

The project is entirely written in C and compiled using a `Makefile`.

To compile the program, simply run:
```bash
cd philo
make
```

This will generate an executable named `philo` inside the `philo/` folder.

Other Makefile rules:
- `make clean` : Removes object files.
- `make fclean`: Removes object files and the `philo` executable.
- `make re`    : Recompiles the whole project.

## Usage

The program runs with the following mandatory and optional arguments:
```bash
./philo number_of_philosophers time_to_die time_to_eat time_to_sleep [number_of_times_each_philosopher_must_eat]
```

- **number_of_philosophers**: The number of philosophers and forks on the table.
- **time_to_die** (in milliseconds): If a philosopher didn't start eating `time_to_die` milliseconds since the beginning of their last meal or the beginning of the simulation, they die.
- **time_to_eat** (in milliseconds): The time it takes for a philosopher to eat. They need to hold two forks during this time.
- **time_to_sleep** (in milliseconds): The time a philosopher will spend sleeping.
- **number_of_times_each_philosopher_must_eat** (optional argument): If all philosophers have eaten at least `number_of_times_each_philosopher_must_eat` times, the simulation stops. If not specified, the simulation stops when a philosopher dies.

### Execution Examples

Run the simulation with 5 philosophers, where they die if they don't eat within 800ms, it takes them 200ms to eat, and 200ms to sleep:
```bash
./philo 5 800 200 200
```
Run the same simulation but stop it once all philosophers have eaten at least 7 times:
```bash
./philo 5 800 200 200 7
```

## Implementation Highlights

- **Threads:** Each philosopher is fundamentally represented as a discrete thread (implemented via `<pthread.h>`).
- **Mutexes:** To safeguard shared resources (the forks, printing logs, and managing status) and prevent deadlocks and data races, the program relies strictly on mutex locks (`pthread_mutex_t`).
- **Time Management:** Precise control overhead on routines relying on custom time polling functions combined with `usleep`.

