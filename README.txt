Diagramming Study

Overview

Viewing images on the terminal

Links

https://naildrivin5.com/blog/2016/12/08/learn-graphviz-and-up-your-diagramming-game.html
https://www.iterm2.com/documentation-images.html
https://www.iterm2.com/utilities/imgcat

digraph classes {
  rankdir=BT
  node[shape=record]

  User           -> ActiveRecordBase
  Address        -> ActiveRecordBase
  Product        -> ActiveRecordBase
  SpecialProduct -> Product
}

Viewing images on the terminal

cat <<'EOF' | dot -T png | imgcat
digraph classes {
  rankdir=BT
  node[shape=record]

  User           -> ActiveRecordBase
  Address        -> ActiveRecordBase
  Product        -> ActiveRecordBase
  SpecialProduct -> Product
}
EOF

cat <<'EOF' | dot -T png | imgcat
digraph g1 {
  // Use the radial layout instead
  // of the hierarchical one
  layout="circo";

  // The meat: these are the dependencies between
  // applications and services
  WMS -> InvLocService
  WMS -> CustomerService
  WMS -> ShippingLabels
  WMS -> ProductService
  WMS -> Checkout
  WMS -> Metrics
  WMS -> AddressService

  Clearance -> OrderService
  Clearance -> InvLocService
  PickAndShip -> PickingService
  PickAndShip -> Metrics
  PickingService -> OrderService
  PickingService -> InvLocService

  Admin -> SchedulingService
  Admin -> OrderService
  Admin -> ShippingLabels
  Admin -> ProductService
  Admin -> CustomerService

  OrderService -> ProductService
  OrderService -> ShippingLabels
  ProductService -> InvLocService

  // This forces a circular layout.
  // The "penwidth" and "arrowhead" settings
  // at the end of this ensure these
  // edges won't be visible.  But, they
  // will ensure the services are arranged
  // in a circle
  WMS ->
    Checkout ->
    InvLocService ->
    AddressService ->
    Metrics ->
    PickAndShip ->
    PickingService ->
    Clearance ->
    OrderService ->
    ShippingLabels ->
    Admin ->
    SchedulingService ->
    CustomerService ->
    ProductService -> WMS [penwidth=0 arrowhead=none];

  // Now, configure visuals for the apps and services.
  // We'll have the user-facing apps use a double circle
  // and the headless services use a single one

  WMS               [ shape=doublecircle];
  Clearance         [ shape=doublecircle];
  PickAndShip       [ shape=doublecircle];
  Admin             [ shape=doublecircle];
  Metrics           [ shape=doublecircle];

  InvLocService     [ shape=circle label="Inventory Locations"];
  PickingService    [ shape=circle label="Picking"];
  Checkout          [ shape=circle label="Financial Transactions"];
  OrderService      [ shape=circle label="Orders"];
  ShippingLabels    [ shape=circle label="Shippinng Labels"];
  SchedulingService [ shape=circle label="Scheduling"];
  CustomerService   [ shape=circle label="Customers"];
  ProductService    [ shape=circle label="Products"];
  AddressService    [ shape=circle label="Addresses"];
}
EOF