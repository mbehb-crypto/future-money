# future-money
the future of tomorrow will always be on the table.  
import hashlib
import time
import json

class PersonalCurrency:
    def __init__(self, owner_name, token_name, initial_supply):
        self.owner = owner_name
        self.token_name = token_name
        self.chain = []
        self.pending_transactions = []
        
        # Create the genesis block (the first block in the chain)
        self.create_block(proof=1, previous_hash='0' * 64)
        
        # Mint initial supply to the owner
        self.mint(owner_name, initial_supply)

    def create_block(self, proof, previous_hash):
        """Creates a new block in the blockchain"""
        block = {
            'index': len(self.chain) + 1,
            'timestamp': time.time(),
            'transactions': self.pending_transactions,
            'proof': proof,
            'previous_hash': previous_hash,
        }
        self.pending_transactions = []
        self.chain.append(block)
        return block

    def mint(self, to_address, amount):
        """Creates new currency tokens"""
        self.pending_transactions.append({
            'from': 'NETWORK',
            'to': to_address,
            'amount': amount
        })
        # In a real system, you would mine a block here to finalize the minting
        print(f"Minted {amount} {self.token_name} to {to_address}")

    def transfer(self, from_address, to_address, amount):
        """Transfers tokens between users"""
        # Simplified: assumes sender has balance
        self.pending_transactions.append({
            'from': from_address,
            'to': to_address,
            'amount': amount
        })
        print(f"Transfer: {amount} {self.token_name} from {from_address} to {to_address}")

    def hash(self, block):
        """Hashes a block for security"""
        encoded_block = json.dumps(block, sort_keys=True).encode()
        return hashlib.sha256(encoded_block).hexdigest()

    def get_last_block(self):
        return self.chain[-1]

# --- THE FUTURE: Personal Currency Creation ---

# 1. User initiates their own currency (e.g., "AliceCoin")
my_currency = PersonalCurrency(owner_name="Alice", token_name="ALC", initial_supply=1000)

# 2. Alice transfers some of her currency to Bob
my_currency.transfer("Alice", "Bob", 50)

# 3. A block is mined to finalize the transaction (1.2.5, 1.4.2)
last_block = my_currency.get_last_block()
new_block = my_currency.create_block(proof=12345, previous_hash=my_currency.hash(last_block))

print("\n--- Blockchain Ledger ---")
print(json.dumps(my_currency.chain, indent=2))
