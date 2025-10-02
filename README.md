import random
import itertools
import operator

# Define operations
ops = {
    '+': operator.add,
    '-': operator.sub,
    '*': operator.mul,
    '/': operator.truediv
}

# Generate numbers based on difficulty
def generate_numbers(difficulty="easy"):
    if difficulty == "easy":
        return [random.randint(1, 9) for _ in range(4)]
    elif difficulty == "medium":
        return [random.randint(1, 15) for _ in range(4)]
    elif difficulty == "hard":
        return [random.randint(1, 20) for _ in range(4)]
    else:
        raise ValueError("Difficulty must be easy, medium, or hard")

# Check if 24 can be made
def can_make_24(nums):
    for num_perm in itertools.permutations(nums):
        for ops_combo in itertools.product(ops.keys(), repeat=3):
            exprs = [
                f"(({num_perm[0]}{ops_combo[0]}{num_perm[1]}){ops_combo[1]}{num_perm[2]}){ops_combo[2]}{num_perm[3]}",
                f"({num_perm[0]}{ops_combo[0]}({num_perm[1]}{ops_combo[1]}{num_perm[2]})){ops_combo[2]}{num_perm[3]}",
                f"{num_perm[0]}{ops_combo[0]}(({num_perm[1]}{ops_combo[1]}{num_perm[2]}){ops_combo[2]}{num_perm[3]})",
                f"{num_perm[0]}{ops_combo[0]}({num_perm[1]}{ops_combo[1]}({num_perm[2]}{ops_combo[2]}{num_perm[3]}))"
            ]
            for expr in exprs:
                try:
                    if abs(eval(expr) - 24) < 1e-6:
                        return True, expr
                except ZeroDivisionError:
                    continue
    return False, None

# Example game loop
def play_game():
    difficulty = input("Choose difficulty (easy/medium/hard): ").lower()
    nums = generate_numbers(difficulty)
    print(f"Your numbers are: {nums}")

    solvable, solution = can_make_24(nums)
    if solvable:
        print("This puzzle CAN be solved!")
        print(f"One solution is: {solution}")
    else:
        print("This puzzle has no solution. Try again!")

if __name__ == "__main__":
    play_game()
