# Buy Option Flow

This sequence diagram illustrates the complete flow when a user purchases options through the SentientTrader contract.

```mermaid
sequenceDiagram
    participant U as User Wallet
    participant ST as SentientTrader
    participant TR as Thetanuts Router
    participant LP as Liquidity Pool
    
    Note over U: Has USDC
    U->>ST: 1. Approve USDC spending
    U->>ST: 2. buyOption(optionToken, amountIn)
    
    ST->>ST: Check authorization
    ST->>ST: Check not paused
    ST->>U: 3. transferFrom(USDC)
    
    ST->>TR: 4. Approve USDC
    ST->>TR: 5. swapExactTokensForTokens()
    TR->>LP: 6. Execute swap
    LP->>TR: 7. Return option tokens
    TR->>ST: 8. Return option tokens
    
    ST->>ST: 9. Validate slippage
    ST->>U: 10. Transfer option tokens
    
    Note over U: Received Option Tokens
    ST->>ST: Emit OptionPurchased event
```

## Step-by-Step Breakdown

1. **Approve USDC**: User grants SentientTrader permission to spend USDC
2. **Call buyOption()**: User initiates the option purchase
3. **Authorization Check**: Contract verifies caller is authorized
4. **Pause Check**: Contract ensures trading is not paused
5. **Transfer USDC**: Contract pulls USDC from user wallet
6. **Approve to Router**: Contract approves Thetanuts router to spend USDC
7. **Execute Swap**: Contract calls router to swap USDC for option tokens
8. **Router Execution**: Router executes the swap with liquidity pools
9. **Receive Tokens**: Option tokens are returned to contract
10. **Slippage Validation**: Contract verifies slippage is within acceptable limits
11. **Transfer to User**: Option tokens are sent to user wallet
12. **Event Emission**: Transaction details are logged on-chain

## Security Checks

- Authorization validation before execution
- Pause mechanism to halt trading if needed
- Slippage protection to prevent unfavorable trades
- Complete event logging for transparency
