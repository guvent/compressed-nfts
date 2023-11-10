## Security Practices

When making a program, it's not enough to make it just work. It's key to consider the potential weak spots where it could break or be misused. Once your program is live, you have no control over the inputs it receives. Your job is to prepare it to handle those inputs securely.

Every program can be broken. It's not a question of "if." Rather, it's a question of "how much effort and dedication would it take." It's your job to make breaking it as tough as possible. This means finding and fixing as many weak points as possible. To do this, you have to think like a hacker asking "How do I break this?".

### Basic Security Checks

Include checks to make sure:

1. The account is owned by your program (Ownership checks)

2. The account has signed a transaction (Signer checks)

3. The account is what you expect it to be (General Account Validation)

4. The user inputs are valid (Data Validation)

### Ownership Checks

Account<'info, T> is a wrapper around AccountInfo that verifies program ownership and deserializes underlying data into the specified account type T. This in turn allows you to use Account<'info, T> to easily validate ownership.

For context, the #[account] attribute implements various traits for a data structure representing an account. One of these is the Owner trait which defines an address expected to own an account. The owner is set as the program ID specified in the declare_id! macro.

```
admin_config: Account<'info, AdminConfig>,
```

### Signer Checks

Anchor makes it easy to perform signer checks by providing the Signer account type. Simply change the authority account’s type in the account validation struct to be of type Signer, and Anchor will check at runtime that the specified account is a signer on the transaction.

```
pub authority: Signer<'info>
```

### General Account Validation

With Anchor, you can specify conditions using the #[account(...)] attribute macro while defining the associated accounts for a particular instruction. This attribute allows you to express constraints on the accounts in a declarative way. These constraints are then checked automatically before the function or instruction is executed.

```
#[account(mut, has_one = owner)]
from: AccountInfo<'info>,
```

### Data Validation

Make sure the data provided by users is valid. For instance, if your game allows users to distribute points to characters, make sure they don't exceed the maximum or their allowed points. These can be achieved easily using if checks as we have done throughout the course.

---

## More Advance Security Checks

### Integer Overflow and Underflow

When using Rust, keep in mind that its integers have a fixed size, and exceeding that size causes the number to wrap around. This is especially crucial when dealing with real value data, like tokens.

For example, a u8 only supports numbers 0-255, so the result of addition that would be 256 would actually be 0, 257 would be 1, etc.

Using checked math like checked_add instead of + is generally a good practice to help you avoid overflow and underflow.

### Reinitialization Attacks

Creating an account and initializing it are two separate actions on Solana. You create an account through the System Program using a command called create_account. During this creation process, you establish how much memory space is needed for the account, how much rent in lamports (Solana's native cryptocurrency) to allocate, and who owns the account.

Once an account is created, it has to be filled with data, a process called "initialization". For example, if you were building a game on Solana, initialization could be setting up a new player's stats for the first time. However, without protective measures, a newly created account could be initialized more than once, which can lead to problems like overwriting existing data.

Use tools like Anchor's init constraint, which ensures an account can only be initialized once. The init constraint must be used in combination with the payer and space constraints. The payer specifies the account paying for the initialization of the new account. The space specifies the amount of space the new account requires, which determines the amount of lamports that must be allocated to the account. The first 8 bytes of data is set as a discriminator that Anchor automatically adds to identify the account type.

### Duplicate Mutable Accounts

If an instruction requires two accounts of the same type, make sure they're different to prevent unexpected changes. Anchor provides a simple way to check this.
```
#[derive(Accounts)]
pub struct Update<'info> {
#[account(constraint = user_a.key() != user_b.key())]
    user_a: Account<'info, User>,
    user_b: Account<'info, User>,
}
```

### Anchor’s seeds and bump constraints

These can be used to validate the Program-Derived Address (PDA), ensuring only the correct accounts are used in instructions.

A PDA is calculated from two things: Seeds (one or more strings, public keys, or integers) and Bump (a number 0-255 that is found through an iterative process to make sure that the derived address is a valid one.) Anchor’s seeds and bump constraints are used in combination to validate that an account is indeed the correct PDA.

Let's consider a scenario where you are running a gaming application on Solana and you want to create a specific account for every player to keep track of their high scores. Here, a PDA would be necessary because the player's account should be predictable (to find and update scores), but no one but the program should be able to write to it (to prevent score tampering).

You can use the player's public key and the string "highscore" as seeds to generate the PDA. You would also calculate a bump to ensure the PDA is valid. In your program, you would use these constraints to validate the PDA when you interact with it.

```
#[account(seeds = ["highscore", player.key().as_ref()], bump = bump)]
player_highscore: Account<'info, HighScore>,
```

Always keep in mind that programming securely means anticipating potential weak points and handling them accordingly. It's not just about making the program work, but also about protecting it from misuse and errors.