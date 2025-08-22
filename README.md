# Enable Strike-through Price On Collection Product Cards

## Getting Help

We strongly recommend contacting our support team for step-by-step guidance. You can:
- Use the **Live chat** button at the bottom left of your screen
- Email us at **aiod-app@dtcdevelopers.org**

If you prefer to follow the setup yourself, use the instructions below.

---

## Setup Instructions

### Step 1: Create the Price Crossout Snippet

1. Navigate to your **store theme code editor**
2. Right-click on the **"snippets"** folder
3. Click **"New file"**
4. Name the file: `aiod-price-crossout.liquid`
5. Paste the following code into the newly created file:

```js
{%- liquid 
  assign target = product 
  assign price = target.price | default: 199 
  assign available = target.available | default: false 
  assign aiod_auto_discounted_price = target.price 
  assign original_display_price = target.price 
-%}

{%- unless target == null -%}
  {%- if available -%}
    <div product-id="{{ product.id }}" 
         aiod-product-collections="{{ product.collections | map: 'id' | join: ',' }}" 
         aiod-product-variants="{{ product.variants | map: 'id' | join: ',' }}" 
         class="aiod-card-price-container price">
      
      <span class="price-item price-item-regular aiod-original-price">
        {% if settings.currency_code_enabled %}
          {{ original_display_price | money_with_currency }}
        {% else %}
          {{ original_display_price | money }}
        {% endif %}
      </span>
      
      <span class="price-item price-item-sale aiod-discounted-price"></span>
      <span class="aiod-discount-badge"></span>
    </div>

    <script>
      function hideOriginalProductPrice{{ product.id }}() {
        const aiodPriceContainer = document.querySelector(".aiod-card-price-container[product-id='{{ product.id }}']");
        const parentContainer = aiodPriceContainer.closest('.card-information');
        
        if(parentContainer){
          // Remove ALL original price elements
          const originalPrices = parentContainer.querySelectorAll('div.price:not(.aiod-card-price-container)');
          originalPrices.forEach(price => price.remove());
        }
        
        const wholeCardParentContainer = aiodPriceContainer.closest('.card-wrapper');
        if(wholeCardParentContainer){
          wholeCardParentContainer.classList.add('aiod-product-card-wrapper');
        }
      }

      // Execute immediately after DOM is ready
      if (document.readyState === 'loading') {
        document.addEventListener('DOMContentLoaded', hideOriginalProductPrice{{ product.id }});
      } else {
        hideOriginalProductPrice{{ product.id }}();
      }
    </script>
  {%- endif -%}
{% endunless %}
```

6. Click **"Save"**

### Step 2: Add the Snippet to Your Theme

#### For Official Shopify Themes:

1. In the same **"snippets"** folder, locate and open `card-product.liquid`
2. Use the search function:
   - **Mac**: Press `Command+F`
   - **Windows**: Press `Ctrl+F`
3. Search for: `render 'price'`
4. You should find 2 instances of this line
5. **Below each line**, paste the following code:

```js
{% render 'aiod-price-crossout', product: card_product %}
```

6. Click **"Save"**

#### For Custom/Third-party Themes:

If you're using a custom or third-party theme, you need to:

1. Identify the file that renders your product collection pages
2. Add the following line in the appropriate location:

```js
{% render 'aiod-price-crossout', product: card_product %}
```

**Need help?** Contact our support team - we'll help you identify the correct file and placement.

---

## Final Step: Configure App Blocks

After completing the code setup:

1. Open the **AIOD app**
2. Click the **"Add"** buttons to add app blocks
3. Add blocks to **all pages** where you want to display strike-through pricing

---

## Support

For any questions or assistance with setup, please contact our support team:

- **Live Chat**: Use the chat button at the bottom left of your screen
- **Email**: aiod-app@dtcdevelopers.org

We're here to help you get set up quickly and correctly!
