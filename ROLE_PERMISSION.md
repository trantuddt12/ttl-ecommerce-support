-- 1. INSERT PERMISSIONS

INSERT INTO permissions (name, description) VALUES

-- USER
('USER_VIEW', 'View users'),
('USER_CREATE', 'Create users'),
('USER_UPDATE', 'Update users'),
('USER_DELETE', 'Delete users'),

-- ROLE
('ROLE_VIEW', 'View roles'),
('ROLE_MANAGE', 'Manage roles'),

-- PRODUCT
('PRODUCT_VIEW', 'View products'),
('PRODUCT_CREATE', 'Create products'),
('PRODUCT_UPDATE', 'Update products'),
('PRODUCT_DELETE', 'Delete products'),
('PRODUCT_PUBLISH', 'Publish products'),

-- CATEGORY
('CATEGORY_VIEW', 'View categories'),
('CATEGORY_MANAGE', 'Manage categories'),

-- BRAND
('BRAND_VIEW', 'View brands'),
('BRAND_MANAGE', 'Manage brands'),

-- ATTRIBUTE
('ATTRIBUTE_VIEW', 'View attributes'),
('ATTRIBUTE_MANAGE', 'Manage attributes'),

-- ORDER
('ORDER_VIEW', 'View orders'),
('ORDER_CREATE', 'Create orders'),
('ORDER_UPDATE', 'Update orders'),
('ORDER_CANCEL', 'Cancel orders'),
('ORDER_APPROVE', 'Approve orders'),
('ORDER_SHIP', 'Ship orders'),
('ORDER_REFUND', 'Refund orders'),

-- PAYMENT
('PAYMENT_VIEW', 'View payments'),
('PAYMENT_CONFIRM', 'Confirm payments'),
('PAYMENT_REFUND', 'Refund payments'),

-- INVENTORY
('INVENTORY_VIEW', 'View inventory'),
('INVENTORY_UPDATE', 'Update inventory'),

-- VOUCHER
('VOUCHER_VIEW', 'View vouchers'),
('VOUCHER_MANAGE', 'Manage vouchers'),

-- REPORT
('REPORT_VIEW', 'View reports'),
('REPORT_EXPORT', 'Export reports'),

-- CUSTOMER
('CUSTOMER_VIEW', 'View customers'),
('CUSTOMER_MANAGE', 'Manage customers'),

-- SYSTEM
('SYSTEM_CONFIG', 'System configuration'),
('AUDIT_VIEW', 'View audit logs');

-- 2. INSERT ROLES
INSERT INTO roles (name, description) VALUES

('ADMIN', 'System administrator with full access'),

('STORE_MANAGER', 'Manage store operations, products, orders'),

('STAFF', 'Operational staff handling orders and inventory'),

('SUPPORT', 'Customer support staff'),

('MARKETING', 'Marketing and promotion management'),

('CUSTOMER', 'End user / customer');

-- 3. ROLE → PERMISSION MAPPING

-- 3.1 ADMIN (ALL PERMISSIONS)
INSERT INTO role_permissions (role_id, permission_id)
SELECT r.id, p.id
FROM roles r, permissions p
WHERE r.name = 'ADMIN';

-- 3.2 STORE_MANAGER

INSERT INTO role_permissions (role_id, permission_id)
SELECT r.id, p.id
FROM roles r
JOIN permissions p ON p.name IN (

'PRODUCT_VIEW','PRODUCT_CREATE','PRODUCT_UPDATE','PRODUCT_DELETE','PRODUCT_PUBLISH',
'CATEGORY_VIEW','CATEGORY_MANAGE',
'BRAND_VIEW','BRAND_MANAGE',
'ATTRIBUTE_VIEW','ATTRIBUTE_MANAGE',
'ORDER_VIEW','ORDER_APPROVE','ORDER_SHIP','ORDER_REFUND',
'INVENTORY_VIEW','INVENTORY_UPDATE',
'REPORT_VIEW','REPORT_EXPORT'
)
WHERE r.name = 'STORE_MANAGER';

-- 3.3 STAFF

INSERT INTO role_permissions (role_id, permission_id)
SELECT r.id, p.id
FROM roles r
JOIN permissions p ON p.name IN (

'ORDER_VIEW','ORDER_UPDATE','ORDER_SHIP',
'INVENTORY_VIEW','INVENTORY_UPDATE',
'PRODUCT_VIEW',
'CUSTOMER_VIEW'
)
WHERE r.name = 'STAFF';

-- 3.4 SUPPORT
INSERT INTO role_permissions (role_id, permission_id)
SELECT r.id, p.id
FROM roles r
JOIN permissions p ON p.name IN (

'ORDER_VIEW',
'CUSTOMER_VIEW',
'CUSTOMER_MANAGE',
'REPORT_VIEW'
)
WHERE r.name = 'SUPPORT';

-- 3.5 MARKETING
INSERT INTO role_permissions (role_id, permission_id)
SELECT r.id, p.id
FROM roles r
JOIN permissions p ON p.name IN (

'PRODUCT_VIEW',
'VOUCHER_VIEW','VOUCHER_MANAGE',
'REPORT_VIEW','REPORT_EXPORT',
'CUSTOMER_VIEW'
)
WHERE r.name = 'MARKETING';

-- 3.6 CUSTOMER

INSERT INTO role_permissions (role_id, permission_id)
SELECT r.id, p.id
FROM roles r
JOIN permissions p ON p.name IN (

'PRODUCT_VIEW',
'ORDER_CREATE','ORDER_VIEW'
)
WHERE r.name = 'CUSTOMER';