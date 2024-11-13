Copyright by Fair Launch Labs(F.L.L.)

Nov.6, 2024

# A new proposal of distributing tokens on blockchain: Proof of Mint

### Abstract

The concept of Fair Mint (or Fair Launch) has gained significant traction within blockchain communities, yet it faces critical issues such as Sybil attacks, insufficient time for building community consensus, fraud, and lack of market value management.

This paper introduces a new solution, Proof of Mint (PoM), inspired by Bitcoin's mining difficulty mechanism. PoM aims to address the fairness concerns by transforming hash power into minting participation levels, thereby mitigating the impact of Sybil attacks and providing ample time for community consensus building.

The proposed PoM is designed to approach ensures a stable minting process, discourages speculative and cheating behavior, incentivizes real user. The paper provides a detailed analysis of the PoM mechanism, including core formulas for calculating the difficulty coefficient, and mint size per Epoch etc.

The paper also presents simulation test parameters and results, showcasing the PoM's effectiveness maintaining a stable minting curve. Additionally, it discusses the total supply calculation based on Eras, Epochs, and reduction factors, as well as the estimated total minting time.

The paper proposes a significant advancement in the Fair Mint paradigm, offering a more equitable and community-driven approach to token minting and distribution. The PoM has the potential to reshape the landscape of decentralized finance by ensuring a fairer and more sustainable minting process.

## 1- Issues

In the past two years, the Fair Mint (or Fair Launch) in the blockchain communities has become extremely popular. Numerous tokens have quickly completed the minting process and were subsequently listed on decentralized exchanges for trading. However, their price trends often exhibit a common pattern: after an initial rapid increase, the prices continue to decline incessantly.

Over two years of observation and research on various Fair Mint platforms and projects, we have identified the following issues:

### 1.1 - Sybil Attack

According to relevant data, the proportion of real participants of such Fair Mint games is as high as 90%. However, it is surprising that over 95% of the tokens are actually minted by a minority of individuals proficient in blockchain technology. Consequently, the vast majority of ordinary real users can only obtain a very small amount of tokens.

This technical method is commonly known as "Sybil Attack", which robs Fair Mint of its "fairness." A few individuals control a large number of tokens obtained at almost zero cost, and they manipulate the market by rapidly driving up the token prices and then selling them out at high prices to reap substantial profits.

### 1.2 - Insufficient time to build consensus

The communities have insufficient time to build consensus. Due to the rapid speed of token minting, the price often begins to decline while the community is still in the process of formation, leading to the disintegration of community consensus and ultimately the dissolution of the community.

### 1.3 - Fraud

Some project parties participate in minting through technical means themselves, creating a false impression of market "excitement" and obtaining low-cost tokens, manipulating the market to gain huge profits, turning Fair Mint into a tool for fraud.

### 1.4 - Lack of Market Value Management(*MVM*)

There is a lack of effective market value management measures. Given the exceptionally fast minting speed and the fact that usually 100% of the tokens are directly (or after the completion of minting) put into circulation, market value management becomes crucial. However, the "ownerless" feature of tokens associated with Fair Mint requires the community to spontaneously organize MVM. Unfortunately, due to the rapid minting speed, MVM often cannot be implemented in time before the collapse of the token price.

### 1.5 - Does liquidity management workable?

Some Fair Mint platforms have introduced liquidity management mechanisms, such as charging a certain fee during minting, which is locked in a smart contract and automatically added to the liquidity pool of decentralized exchanges after certain conditions are met (such as reaching a target amount or completing all minting).

However, we have observed that in the initial rapid price increase of these projects, the funds in the liquidity pool account for a very low proportion of the total liquidity pool; and when the price falls, the funds in the liquidity pool are far from sufficient to withstand the sell-off.

### 1.6 - Does Time Locker workable?

Some smart contracts introduce Time Locker to prevent Sybil-attacks, which can indeed prevent batch minting using the same account, but they still cannot prevent those who implement batch minting using different accounts.

### 1.7 - Mempool and *MEV*

The mainstream blockchains are mostly based on Ethereum EVM, and the `mempool` of EVM allows a large number of Bots to "peek" at users' intentions before the new block is packed and take the lead in on-chain actions, such as minting and trading.
We have observed that there is a large amount of such behavior on both Ethereum (with a block packing interval of about 12 seconds) and Layer 2 networks like Arbitrum (with a block packaging interval of about 0.25 seconds). This type of behavior is well known as: `Maximal Extractable Value (MEV)`. It is estimated that since January 1, 2020, the total amount of `MEV` has exceeded 730 million US dollars, most of which has flowed to robots and miners. The existence of MEV is technically driven and, although understandable, it greatly affects the fairness of on-chain actions.

We believe that effectively preventing technical means such as Sybil attacks and giving sufficient time for the establishment of community consensus have become the most serious challenges. Although many Fair Mint platforms have realized the seriousness of this issue and have tried to take corresponding measures to prevent various types of cheating, the actual effects achieved are quite limited.

To effectively prevent Sybil attacks, some platforms or projects have adopted tech such as `KYC authentication` or relying on servers controlled by the project team to provide `authorized signatures` requiring minters to have this signature to mint. However, these methods are too centralized and therefore difficult to be accepted widely and supported from the community.

## 2- Proposal

We believe that, given the current blockchain technology, it is very difficult to completely eliminate these issues. However, some new mechanisms can alleviate these problems and enhance the fairness of Fair Mint and build better community.

This paper proposes the following solutions:

### 2.1 - The Difficulty Mechanism of Bitcoin Mining

This proposal draws on the difficulty mechanism of Bitcoin mining, so before introducing this proposal, let's briefly introduce the Bitcoin mining mechanism.

The difficulty mechanism of Bitcoin mining is a core component of the Bitcoin network. It ensures that the minting progress is maintained at a stable rate of approximately one block every 10 minutes on a decentralized network. This mechanism is designed to adapt to changes in the network hash power at different times, with the aim of keeping the generation speed of new blocks relatively stable regardless of how the nodes joining the network.

The adjustment of Bitcoin mining difficulty is achieved through an automatic algorithm that adjusts the difficulty approximately every two weeks (2016 blocks). This cycle is designed based on the target block generation time of the Bitcoin network, which is one block every 10 minutes. If the average generation time of new blocks in the past 2016 blocks is less than 10 minutes, the difficulty will increase to ensure that the future block generation time returns back to 10 minutes. Conversely, if the block generation time is more than 10 minutes on average, the difficulty will decrease.

The specific calculation of the difficulty is based on the comparison between the generation time of the previous 2016 blocks and the target time (20160 minutes, i.e., two weeks). The difficulty adjustment formula increases or decreases the difficulty based on the ratio of actual time to target time. This adjustment is continuous to ensure that the Bitcoin network can adapt to the constantly changing hash power, whether miners joining or leaving, or high hash power mining hardware involving.

Furthermore, the adjustment of difficulty is also subject to a maximum change limit, that is, the difficulty cannot exceed 4 times the previous difficulty in any single adjustment. This is to prevent sudden large fluctuations in difficulty from impacting the network.

As the Bitcoin network develops, the mining difficulty continues to increase, mainly due to the growing hash power participating in mining. As more and more miners join the network and mining hardware technology improves, the total computing power for mining continues to rise, which increases the difficulty for individual miners or mining pools to discover new blocks. Therefore, the increase in mining difficulty reflects the growth of hash power in the Bitcoin network and also ensures that the issuance rate of Bitcoin remains stable.

The Bitcoin mining difficulty mechanism is key to the adaptability and stability of the Bitcoin network. It ensures that the generation rate of Bitcoin blocks remains at the designed target level by dynamically adjusting the difficulty value, while adapting to changes in network hash power.

#### Key code of aboved difficulty rules in bitcoin code(c++ version).

```cpp
/* Calculate the difficulty for a given block index.
 */
double GetDifficulty(const CBlockIndex& blockindex)
{
    int nShift = (blockindex.nBits >> 24) & 0xff;
    double dDiff =
        (double)0x0000ffff / (double)(blockindex.nBits & 0x00ffffff);

    while (nShift < 29)
    {
        dDiff *= 256.0;
        nShift++;
    }
    while (nShift > 29)
    {
        dDiff /= 256.0;
        nShift--;
    }

    return dDiff;
}
```

Bitcoin github repo: [Source code](https://github.com/bitcoin/bitcoin/blob/1dda1892b6bcc3d4f9678960cc9e9920f491e87e/src/rpc/blockchain.cpp#L87C1-L107C2)

```cpp
unsigned int GetNextWorkRequired(const CBlockIndex* pindexLast, const CBlockHeader *pblock, const Consensus::Params& params)
{
    assert(pindexLast != nullptr);
    unsigned int nProofOfWorkLimit = UintToArith256(params.powLimit).GetCompact();

    // Only change once per difficulty adjustment interval
    if ((pindexLast->nHeight+1) % params.DifficultyAdjustmentInterval() != 0)
    {
        if (params.fPowAllowMinDifficultyBlocks)
        {
            // Special difficulty rule for testnet:
            // If the new block's timestamp is more than 2* 10 minutes
            // then allow mining of a min-difficulty block.
            if (pblock->GetBlockTime() > pindexLast->GetBlockTime() + params.nPowTargetSpacing*2)
                return nProofOfWorkLimit;
            else
            {
                // Return the last non-special-min-difficulty-rules-block
                const CBlockIndex* pindex = pindexLast;
                while (pindex->pprev && pindex->nHeight % params.DifficultyAdjustmentInterval() != 0 && pindex->nBits == nProofOfWorkLimit)
                    pindex = pindex->pprev;
                return pindex->nBits;
            }
        }
        return pindexLast->nBits;
    }

    // Go back by what we want to be 14 days worth of blocks
    int nHeightFirst = pindexLast->nHeight - (params.DifficultyAdjustmentInterval()-1);
    assert(nHeightFirst >= 0);
    const CBlockIndex* pindexFirst = pindexLast->GetAncestor(nHeightFirst);
    assert(pindexFirst);

    return CalculateNextWorkRequired(pindexLast, pindexFirst->GetBlockTime(), params);
}
```

Bitcoin github repo: [Source code](https://github.com/bitcoin/bitcoin/blob/1dda1892b6bcc3d4f9678960cc9e9920f491e87e/src/pow.cpp#L14)

### 2.2- Proof of Mint(PoM)

If we replace Bitcoin's hash calculations with the act of minting, effectively transforming hash power into the level of participation in minting, we arrive at our proposed solution, known as **Proof of Mint**, or **PoM** for short.

Assuming we deploy **PoM** on Ethereum, which has updated to a **Proof of Stake (PoS)** consensus with an average block time of approximately **12 seconds**. To achieve stability in the minting process, we have designed the following plan:

*   The entire minting progress is divided into several **Eras**, each **Era** containing several **Epochs**.

*   Each **Epoch** has a difficulty-coefficient (the initial **Epoch**'s difficulty-coefficient is **1**), which is determined by the actual running time of the previous **Epoch** and determines the current block's mint-size.

*   The **target-mint-size-of-epoch** and **base-mint-size-of-epoch** in each **Epoch** within an **Era** decrease according to a fixed **reduction-factor**.

*   Each minting requires a fixed fee, which does not change with the decreasing mint-size of **Era** and **Epoch**.

### 2.3- Example:

In the 1st **Era**, the **target-mint-size-of-epoch** is **100,000 tokens**, the **base-mint-size-of-epoch** is **100 tokens**, the **target-mint-time-per-epoch** is **10 minutes**, and the minting fee is **0.1 ETH**.

On the 10th **Epoch**, if **100,000 tokens** were minted in 3 minutes with a **difficulty-coefficient** of **1.5**, then the 11th **Epoch**'s  **difficulty-coefficient** will automatically increase to **2.55**, and the **mint-size-per-minting** will automatically decrease to **100 / 2.55 = 39.215686275 tokens**, with the cost per token to **0.1ETH / 39.215686275 tokens = 0.00255 ETH**.

The increase in cost and decrease in mint-size can affect the enthusiasm for participation in minting.

Affected by the decrease in mint-size and the decline in enthusiasm, assume that completing the 11th **Epoch** takes **600 minutes** (10 hours), then the 12th **Epoch**'s **difficulty-coefficient** will decrease to **0.2**, and the **mint-size-per-minting** will increase to **100 tokens / 0.2 = 500 tokens**, with the cost per token being **0.1/500 = 0.0002 ETH**.

Additionally, the **base-mint-size-of-epoch** decreases progressively. Assuming a **reduction-factor** of **3/4**:

*   1st **Era**, the **target-mint-size-of-epoch** is **100,000 tokens**, and the **base-mint-size-of-epoch** is **100 tokens**.

*   2nd **Era**, the **target-mint-size-of-epoch** is **100,000 \* 3/4 = 75,000 tokens**, and the **base-mint-size-of-epoch** is **75 tokens**.

*   3rd **Era**, the **target-mint-size-of-epoch** is **75,000 \* 3/4 = 56,250 tokens**, and the **base-mint-size-of-epoch** is **56.25 tokens**.

*   ...

### 2.4- Scenario Analysis

#### 2.4.1- Scenario 1

If `Bots` participate in the minting, they will find that the faster the minting speed, the fewer tokens they receive, and the higher the cost. Contrary to many methods that try to prevent `Bots` from participating minting, this proposal does not prevent Bots from batch minting. However, the high cost and low yield will stop Bots.

#### 2.4.2- Scenario 2

If `Bots` monitor the minting time of the previous `Epoch` to calculate the difficulty of the next `Epoch` and valuate whether to participate or not, then all Bots must have their own strategies. Otherwise, convergent strategies will lead to all Bots crowding and causing a significant decrease in yield and a substantial increase in costs. Because the yield of Bots do not depend on the "speed" of minting, but more on the "guesswork" of the behavior of other Bots, greatly increasing the strategic difficulty for Bots.

> #### **Driven by the principle of maximum benefits, the final minting speed will tend to the target setting, and achieving a Nash equilibrium**.

This mechanism effectively incentivizes early participants and is very friendly to community members who keep attention, while being unfriendly to speculators and those who cheat through technical tricks.

> We have coded `PoM` mining algorithm on the Ethereum testnet `sepolia`(`solidity`) and the `Solana`(`rust/anchor`) Devnet, and will provide front-end `Typescript` scripts for community testing, while also open-sourcing `python` language simulation code.

## 3. Calculations

### 3.1 - Formulas

#### 3.1.1 - Difficulty Coefficient of Current Epoch

*   *d*: Difficulty coefficient
*   *d'*: Difficulty coefficient of previous epoch
*   *Ne*: Elapsed Blocks numbers Of passed Epoch
*   *Nt*: Target Number Of Blocks Per Epoch

```math
d = d' * (2 - \frac{N_e}{N_t})
```

For blockchains that can accurately obtain block timestamps (such as `Solana`), the above `number of blocks` can be replaced with `timestamp`.

**Example:**

In the example above, the target minting time of each `Epoch` is 10 minutes. On the 10th `Epoch`, minting took 3 minutes with a `difficulty-coefficient` of `1.5`, then the new `difficulty-coefficient` for the 11th `Epoch` will be adjusted to: `1.5 * (2 - 3 / 10) = 2.55`.

#### 3.1.2 - Base Mint size per Minting of the Current Era(Mb)

*   *Mb*: Base mint size per minting of current Era
*   *M0*: Base mint size per minting of the Genesis Era
*   *f*: Reduction factor
*   *e*: Current era

```math
M_b = M_0 * f^{e-1}
```

**Reduction factor**

*   `f=0.5` means halving each Era
*   `f=2/3` means a 33.33% reduction each Era
*   `f=3/4` means a 25% reduction each Era
*   `f=4/5` means a 20% reduction each Era
*   `f=5/6` means a 1/6 reduction each Era. etc.

#### 3.1.3 - Target Mint Size per Epoch of current Era

*   *T*: Target mint size per Epoch of the current Era
*   *T0*: Target mint size per Epoch of the Genesis Era
*   *f*: Reduction factor
*   *e*: Current era

```math
 T = T_0 * f^{e-1}
```

#### 3.1.4 - Mint Size per Minting of current Epoch

*   *M*: Mint size per Minting of current Epoch
*   *Mb*: Base mint size per minting of current era
*   *d*: Difficulty coefficient

```math
M = \frac{M_b}{d}
```

**Example:**

In the example above, the `base mint size per minting` of the current Era is `100 tokens`, and the `difficulty-coefficient` is `2.55`, then the minting reward will be: `100 / 2.55 = 39.216 tokens`.

**Note:**

To prevent the `difficulty-coefficient` from being too small, resulting in an excessively large minting reward, even larger than the `target mint size of epoch` (i.e., minting all the target amount of the current `Epoch` at once), a minimum `difficulty-coefficient` of `0.2` is set. At the same time, to prevent the above situation, when initializing the system parameters, it is necessary to ensure: `Mb  < target mint size per Epoch / 5`.

### 3.2 - Explanation

#### 3.2.1

If the `T` is not an integer multiple of `M`, adjustments need to be made to the `M`, that is:

```math
M_a = \frac{T}{\lfloor\frac{T}{M}\rfloor + 1}
```

**Example**
If `d`(difficulty-coefficient) is `1.69`, the `T` is `10,000 tokens`, and the `Mb` is `525 tokens`. The `M` would be `525 / 1.69 = 310.650887574` tokens(see.3.1.4). Based on this mint size, the number of minting instances would be `10,000 / 310.650887574 = 32.19 times`. Since there is a fractional number of instances, we need to round down `32.19` to `32` and add `1`, making it `33 times`. Consequently, the adjusted mint size per minting(`Ma`) is: `10,000 / 33 = 303.03 tokens`.

#### 3.2.2

If the `T` is an integer multiple of the `M`, no adjustments are needed as described above.

#### 3.2.3

`T` and `M` are decreasing exponentially with each `Era`, but the ratio between the two remains constant, regardless of the `Era`.

```math
\because T=T_0*f^{e-1}
```

```math
\because M = \frac{M_b}{d}=\frac{M_0 * f^{e-1}}{d}
```

```math
\therefore \frac{T}{M} = \frac{T_0}{M_0} * d
```

#### 3.2.4

If the *`Nt`* is set to 10 minutes, then the theoretical target interval time for each minting is: 600 seconds / 33 = 18.18 seconds per instance.

If the interval time is longer, it means that completing all mints in an `Epoch` takes longer than planned (10 minutes), which implies fewer miners, and in this case, the difficulty decreases, and the reward per minting increases.

Conversely, if the interval time is shorter, it indicates that completing an `Epoch` of mining takes less time than planned (10 minutes), which implies more miners, and in this case, the difficulty increases, and the reward per minting decreases.

### 3.3- Key code for calculating mint size of epoch (rust)

```rust
fn get_mint_size(config_data: &ConfigData, era: u32, elapsed_seconds_epoch: i64, previous_difficulty_coefficient: f64, target_mint_size_epoch: f64) -> (f64, f64) {
  let _difficulty_coefficient = if 2 * config_data.target_seconds_per_epoch > elapsed_seconds_epoch as u64 {
      let coefficient = previous_difficulty_coefficient * (2.0 - (elapsed_seconds_epoch as f64) / config_data.target_seconds_per_epoch as f64);
      coefficient
  } else {
      MIN_DIFFICULTY_COEFFICIENT
  };

  let difficulty_coefficient = if _difficulty_coefficient < MIN_DIFFICULTY_COEFFICIENT {
      MIN_DIFFICULTY_COEFFICIENT
  } else {
      _difficulty_coefficient
  };

  let reduce_power = config_data.reduce_ratio.powf((era - 1) as f64);

  let mut mint_size = config_data.initial_mint_size * reduce_power / difficulty_coefficient;
  if target_mint_size_epoch % mint_size > 0.0 {
    mint_size = target_mint_size_epoch / ((target_mint_size_epoch / mint_size).trunc() + 1.0);
  }

  if mint_size > target_mint_size_epoch {
      mint_size = target_mint_size_epoch;
      let mut new_difficulty_coefficient = config_data.initial_mint_size * reduce_power / mint_size;

      new_difficulty_coefficient = if new_difficulty_coefficient < MIN_DIFFICULTY_COEFFICIENT {
          MIN_DIFFICULTY_COEFFICIENT
      } else {
          new_difficulty_coefficient
      };

      return (mint_size, new_difficulty_coefficient);
  }

  (mint_size, difficulty_coefficient)
}
```

## 4. Testing and Evaluation

Below are the parameters for the simulation test:

*   Minting interval time range: Randomly between 0-30 seconds
*   Total number of Eras: 10
*   Number of Epochs per Era: 20
*   Minimum difficulty coefficient: 0.2
*   Reduction coefficient: 3/4
*   Base mint size per minting in the Genesis Era: 50
*   Target mint size per Epoch in the Genesis Era: 200

### Minting Rewards

![Epoch Actual Minting Yield vs. Target Minting Yield](https://live.staticflickr.com/65535/54094776890_503884aecf_o.png)
The orange line is the `actual mint size of the current Epoch`, and the blue line represents the `target mint size per minting`, showing a trend of a 25% reduction per Era.

![Actual Minting Reward vs. Target Minting Reward](https://live.staticflickr.com/65535/54095066420_d4262b9e5e_o.png)
The orange line is the `actual mint size`, and the blue line is the `target mint size`.

![Total Minting Curve](https://live.staticflickr.com/65535/54095071645_b6b807f7e9_o.png)
Total minting volume.

![Simulation Results on Ethereum Chain](https://live.staticflickr.com/65535/54094891458_1a1e5df73a_o.png)
The above are simulation results on the Ethereum chain.

## 5. Deployment Parameter

### 5.1 Total Supply

The total supply of PoM scheme is different from the manual setting of the total supply of many tokens. It is calculated based on `Era`, `Epoch`, and the `reduction factor`.

There are four parameters that determine the total supply:

*   *E*: `Total Eras`
*   *C*: `Epoches per Era`
*   *S*: `Initial target mint size per epoch`
*   *R*: `Reduce factory by era`

**Calculation**

```math
TotalSupply = \sum_{i=1}^{E}(C \cdot S \cdot R^{i-1})
```

**Example:** E=15, C=10, S=100,000 coins, R=0.75

**The total yield is:** `3,946,546.155959368` tokens

[Click here to open Wolfram calculation](https://www.wolframalpha.com/input?i=sum+10*100000*0.75%5E%28i-1%29%2C+i+%3D+1+to+15), you can use `wolfram` to calculate the total supply easily.

### 5.2 Estimated Total Minting Time:

**Note:** The actual total minting time will differ from the estimated total minting time.

The following formula is for calculating the estimated total minting time.

*   *E*: `Total Eras`
*   *C*: `Epoches per Era`
*   *B*: `Target blocks per epoch`
*   *t*: `Seconds per block`

**Calculation**

```math
TotalEstimatedTime = E \cdot C \cdot B \cdot t
```

**Example:** E=15, C=10, B=50, t=12 seconds

**The estimated total minting time:** 15 \* 10 \* 50 \* 12 = 90,000 seconds = 25 hours

### 5.3

If the target total supply is 21 million, there can be (but not limited to) the following parameter combinations:

| E  | C   | S     | R    | B    | t  | Total Supply  | Estimated Minting Days |
| -- | --- | ----- | ---- | ---- | -- | ------------- | ---------------------- |
| 13 | 600 | 9000  | 0.75 | 2000 | 12 | 21 million    | 2166.666667            |
| 11 | 500 | 11000 | 0.75 | 500  | 12 | 21.07 million | 381.9444444            |

### 5.4 Mint Size of Genesis Era(M0)

It can be calculated through the following 4 parameters:

*   *S*: `Initial target mint size per epoch`
*   *I*: `Mint interval`
*   *B*: `Target blocks per epoch`
*   *t*: `Seconds per block`

**Calculation**

```math
M_0 = \frac{S \cdot I}{B \cdot t}
```

> What would happen if the `Mint Size of Genesis Era` is not calculated according to the above formula but is set to a very large or very small value?
> As `Epoch` passes, the amount of each minting will eventually converge due to the adjustment of the difficulty coefficient, so this parameter has no long-term impact. However, this setting can be used as a pre-mining for the Genesis block and early blocks.

Note: `M0` should not exceed `S`, otherwise, the total supply will be magnified.

## 6. Attacks prevention

### 6.1 - Mempool Attacks

Simulating the result of sending over 200 transactions in a single block:

| height | tx id  | era | epoch | difficulty | elapsed blocks | mint size   | total minted |
| ------ | ------ | --- | ----- | ---------- | -------------- | ----------- | ------------ |
| 0      | 1-4    | 1   | 0     | 1          | 0              | 50          | 200          |
| 1      | 5-12   | 1   | 1     | 1.96667    | 1              | 25.42368572 | 200          |
| 0      | 13-28  | 1   | 2     | 3.93334    | 0              | 12.71184286 | 200          |
| 0      | 29-60  | 1   | 3     | 7.86668    | 0              | 6.355921431 | 200          |
| 0      | 61-120 | 1   | 4     | 14.42229   | 0              | 3.466855818 | 200          |

The above test shows that if a user sends mass transactions in the same block and all are confirmed, the minting cost per epoch will double, and the total amount per epoch is limited, in the test case, to 200 units.

### 6.2 - Mint tips

Mempool attacks will lead to increasing mint costs (Gas), but as the Gas fees on the Ethereum chain decrease, this cost may become negligible, so it is possible to pay some ETH as a tip for minting.
