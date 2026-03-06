# Secure Cross-Module Checkout

## Problem/Feature Description

We have a modular e-commerce system with two main features: `Orders` and `Inventory`. When a customer places an order, the `Orders` system needs to verify if the requested items are available in stock before finalizing the transaction.

The `Inventory` module is complex and its internal database schema and data management methods change frequently. We want to ensure that the `Orders` module is as loosely coupled as possible to the `Inventory` module. Specifically, we must avoid a "leaky" design where the `Orders` team has to understand the internal data structure or implementation details of the `Inventory` team.

Your task is to implement the stock-check step in the `Orders` module. This interaction should be stable, encapsulated, and should not create a fragile dependency on the internal workings of the other module.

## Input Files

The following file is provided for reference:

=============== FILE: src/Inventory/InventoryService.ts ===============
export class InventoryService {
  constructor(private repo: any) {}

  async checkStock(itemId: string, quantity: number): Promise<boolean> {
    const item = await this.repo.findItem(itemId);
    return item && item.quantity >= quantity;
  }
}

## Output Specification

- Implement the checkout step in the `Orders` module.
- Demonstrate how the `Orders` module interacts with the `Inventory` module.
- The interaction must occur through a stable public interface or service class, not internal implementations.
- The `Orders` module should NOT have any direct access to the `Inventory` module's internal repository or data models.
- Show the directory structure and code for the communication between the two modules.
