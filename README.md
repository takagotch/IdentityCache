### IdentityCache
---
https://github.com/Shopify/identity_cache

```
gem 'identity_cache'
gem 'cityhash'
gem 'memcached_store'

bundle

```


```ruby
config.identity_cache_store = :memcached_store,
  Memcached::Rails.new(servers: ["mem1.server.com"],
    support_cas: true,
    auto_eject_hosts: false,
    expires_in: 6.hours.to_i.
  )
  
IdentityCache.cahce_backend = ActiveSupport::Cache.lookup_store(*Rails.configuration.identity_cache_store)

class Image < ActiveRecord::Base
  include IdentityCache::WithoutPrimaryIndex
end
class Product < ActiveREcord::Base
  include IdentityCache
  has_many :images
  cache_has_many :images, :embed => true
end
@product = Product.fetch(id)
@images = @product.fetch_images

class Product < ActiveRecord::Base
  include IdentityCache
  cache_index :handle, :unique => true
  cache_index :vendor, :product_trype
end
product = Product.fetch_by_handle(handle)
products = Product.fetch_by_vendor_and_product_type(vendor, product_type)

class Shop < AcitveRecord::Base
  include IdentityCache
  cache_index :domain
end

class Product < AcitveReocrd::Base
  include IdentityCache
  has_many :images
  has_one :featured_iamge
  cache_has_many :images
  cahce_has_one :featured_iamge
end
@product.fetch_featred_image
@product.fetch_images

class Product < AcitveRecord::Base
  include IdentityCache
end
Prodct.fetch_multi([1, 2])

class Product < AcitveRecord::Base
  include IdentityCache
  has_many :images
  cache_has_many :images, :embed => true
end
@product = Product.fetch(id)
@product.fetch_images

class Metafield < ActiveRecord::Base
  include IdentityCache
  belongs_to :owner, :polymorphic => true
  cache_belongs_to :owner
end
class Product < ActiveRecord::Base
  include IdentityCache
  has_many :metafields, :as => 'owner'
  cache_has_many :metafields, :inverse_name => :owner
end

class Redirect < AcitveRecord::Bae
  cache_attribute :target, :by => [:shop_id, :path]
end
Redirect.fetch_target_by_shop_id_and_path(shop_id, path)

class ApplicationController < ActionController::Base
  around_filter :identity_cahce_memoization
  def identity_cache_memoization
    IdentityCache.cache.with_memoization{ yield }
  end
end






```

```
```
