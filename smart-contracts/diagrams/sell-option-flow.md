# Sell Option Flow

This sequence diagram illustrates the complete flow when a user sells options through the SentientTrader contract.

```mermaid
sequenceDiagram
    participant U as User Wallet
    participant ST as SentientTrader
    participant TR as Thetanuts Router
    participant LP as Liquidity Pool
    
    Note over U: Has Option Tokens
    U->>ST: 1. Approve option token spending
    U->>ST: 2. sellOption(optionToken, amountIn)
    
    ST->>ST: Check authorization
    ST->>ST: Check not paused
    ST->>U: 3. transferFrom(option tokens)
    
    ST->>TR: 4. Approve option tokens
    ST->>TR: 5. swapExactTokensForTokens()
    TR->>LP: 6. Execute swap
    LP->>TR: 7. Return USDC
    TR->>ST: 8. Return USDC
    
    ST->>ST: 9. Validate slippage
    ST->>U: 10. Transfer USDC
    
    Note over U: Received USDC
    ST->>ST: Emit OptionSold event
```

## Step-by-Step Breakdown

1. **Approve Option Tokens**: User grants SentientTrader permission to spend option tokens
2. **Call sellOption()**: User initiates the option sale
3. **Authorization Check**: Contract verifies caller is authorized
4. **Pause Check**: Contract ensures trading is not paused
5. **Transfer Tokens**: Contract pulls option tokens from user wallet
6. **Approve to Router**: Contract approves Thetanuts router to spend option tokens
7. **Execute Swap**: Contract calls router to swap option tokens for USDC
8. **Router Execution**: Router executes the swap with liquidity pools
9. **Receive USDC**: USDC is returned to contract
10. **Slippage Validation**: Contract verifies slippage is within acceptable limits
11. **Transfer to User**: USDC is sent to user wallet
12. **Event Emission**: Transaction details are logged on-chain

## Security Checks

- Authorization validation before execution
- Pause mechanism to halt trading if needed
- Slippage protection to prevent unfavorable trades
- Complete event logging for transparency
