# PostgreSQL Database Schema Documentation for JOB-ADS

## Users Table
- **Table Name**: `users`
- **Description**: Stores user information.
- **Fields**:
  - `id`: SERIAL PRIMARY KEY
  - `email`: VARCHAR(255) UNIQUE NOT NULL
  - `password`: VARCHAR(255) NOT NULL
  - `name`: VARCHAR(255)
  - `phone`: VARCHAR(15)
  - `profile_picture`: VARCHAR(255)
  - `user_type`: ENUM('admin', 'user') NOT NULL
  - `verification_status`: BOOLEAN DEFAULT FALSE
  - `created_at`: TIMESTAMP DEFAULT CURRENT_TIMESTAMP
  - `updated_at`: TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP

## Listings Table
- **Table Name**: `listings`
- **Description**: Contains job listings.
- **Fields**:
  - `id`: SERIAL PRIMARY KEY
  - `user_id`: INT REFERENCES users(id) ON DELETE CASCADE
  - `title`: VARCHAR(255) NOT NULL
  - `description`: TEXT
  - `category`: VARCHAR(255)
  - `condition`: ENUM('new', 'used') NOT NULL
  - `price`: DECIMAL(10, 2) NOT NULL
  - `location`: VARCHAR(255)
  - `images`: JSONB
  - `status`: ENUM('active', 'inactive') DEFAULT 'active'
  - `created_at`: TIMESTAMP DEFAULT CURRENT_TIMESTAMP
  - `updated_at`: TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP

## Orders Table
- **Table Name**: `orders`
- **Description**: Records purchase orders.
- **Fields**:
  - `id`: SERIAL PRIMARY KEY
  - `buyer_id`: INT REFERENCES users(id) ON DELETE CASCADE
  - `seller_id`: INT REFERENCES users(id) ON DELETE CASCADE
  - `listing_id`: INT REFERENCES listings(id) ON DELETE CASCADE
  - `total_price`: DECIMAL(10, 2) NOT NULL
  - `status`: ENUM('pending', 'completed', 'canceled') DEFAULT 'pending'
  - `created_at`: TIMESTAMP DEFAULT CURRENT_TIMESTAMP
  - `updated_at`: TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP

## Order_items Table
- **Table Name**: `order_items`
- **Description**: Items within an order.
- **Fields**:
  - `id`: SERIAL PRIMARY KEY
  - `order_id`: INT REFERENCES orders(id) ON DELETE CASCADE
  - `product_id`: INT REFERENCES listings(id) ON DELETE CASCADE
  - `quantity`: INT NOT NULL
  - `price`: DECIMAL(10, 2) NOT NULL

## Messages Table
- **Table Name**: `messages`
- **Description**: Contains user messages.
- **Fields**:
  - `id`: SERIAL PRIMARY KEY
  - `sender_id`: INT REFERENCES users(id) ON DELETE CASCADE
  - `recipient_id`: INT REFERENCES users(id) ON DELETE CASCADE
  - `conversation_id`: INT REFERENCES conversations(id) ON DELETE CASCADE
  - `content`: TEXT NOT NULL
  - `read`: BOOLEAN DEFAULT FALSE
  - `created_at`: TIMESTAMP DEFAULT CURRENT_TIMESTAMP

## Conversations Table
- **Table Name**: `conversations`
- **Description**: Active conversations between users.
- **Fields**:
  - `id`: SERIAL PRIMARY KEY
  - `user1_id`: INT REFERENCES users(id) ON DELETE CASCADE
  - `user2_id`: INT REFERENCES users(id) ON DELETE CASCADE
  - `last_message`: TIMESTAMP DEFAULT CURRENT_TIMESTAMP
  - `updated_at`: TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP

## Reviews Table
- **Table Name**: `reviews`
- **Description**: User reviews for listings.
- **Fields**:
  - `id`: SERIAL PRIMARY KEY
  - `reviewer_id`: INT REFERENCES users(id) ON DELETE CASCADE
  - `reviewed_user_id`: INT REFERENCES users(id) ON DELETE CASCADE
  - `listing_id`: INT REFERENCES listings(id) ON DELETE CASCADE
  - `rating`: INT CHECK (rating >= 1 AND rating <= 5)
  - `comment`: TEXT
  - `created_at`: TIMESTAMP DEFAULT CURRENT_TIMESTAMP

## Payments Table
- **Table Name**: `payments`
- **Description**: Payment records for orders.
- **Fields**:
  - `id`: SERIAL PRIMARY KEY
  - `order_id`: INT REFERENCES orders(id) ON DELETE CASCADE
  - `amount`: DECIMAL(10, 2) NOT NULL
  - `payment_method`: ENUM('credit_card', 'paypal', 'bank_transfer')
  - `status`: ENUM('completed', 'pending', 'failed') DEFAULT 'pending'
  - `transaction_id`: VARCHAR(255) UNIQUE

## Wishlist Table
- **Table Name**: `wishlist`
- **Description**: User's saved listings.
- **Fields**:
  - `id`: SERIAL PRIMARY KEY
  - `user_id`: INT REFERENCES users(id) ON DELETE CASCADE
  - `listing_id`: INT REFERENCES listings(id) ON DELETE CASCADE
  - `created_at`: TIMESTAMP DEFAULT CURRENT_TIMESTAMP

## Categories Table
- **Table Name**: `categories`
- **Description**: Listing categories.
- **Fields**:
  - `id`: SERIAL PRIMARY KEY
  - `name`: VARCHAR(255) NOT NULL
  - `slug`: VARCHAR(255) UNIQUE NOT NULL

## Notifications Table
- **Table Name**: `notifications`
- **Description**: User notifications.
- **Fields**:
  - `id`: SERIAL PRIMARY KEY
  - `user_id`: INT REFERENCES users(id) ON DELETE CASCADE
  - `type`: VARCHAR(255) NOT NULL
  - `message`: TEXT NOT NULL
  - `read`: BOOLEAN DEFAULT FALSE
  - `created_at`: TIMESTAMP DEFAULT CURRENT_TIMESTAMP

## Relationships
- A `user` can have multiple `listings`, `orders`, `messages`, `reviews`, `wishlist` items, and `notifications`.
- Each `listing` is linked to a `user` and can have multiple `order_items`.
- Each `order` is linked to a `buyer` and a `seller` (both users) and can contain multiple `order_items`.
- Each `message` belongs to a `sender`, a `recipient`, and a `conversation`.
- Each `review` is written by a `reviewer` for a `reviewed_user` associated with a `listing`.
- Each `payment` corresponds to an `order`.
- Each `wishlist` entry links a `user` to a `listing`.
- Categories link to listings through the `category` field in `listings`. 

