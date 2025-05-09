# Custom Weapons Integration Guide

This guide explains how to add custom weapons directly into weapon categories in vMenu without using the addon weapons submenu.

## Adding a Custom Weapon

### Step 1: Add the Permission
First, add your weapon's permission to `SharedClasses/PermissionsManager.cs`:

```csharp
// Find the Permission enum and add your weapon permission
// Custom Weapons
WPDeagle,
// Add your new weapon permission here, for example:
// WPMyCoolWeapon,
```

### Step 2: Add the Weapon to CustomWeapons
Open `vMenu/data/ValidWeapon.cs` and locate the `ValidWeapons` static constructor. Add your new weapon:

```csharp
static ValidWeapons()
{
    // Existing Desert Eagle example
    var deagleHash = (uint)GetHashKey("weapon_deagle");
    CustomWeapons["weapon_deagle"] = new ValidWeapon
    {
        Hash = deagleHash,
        Name = "Desert Eagle",
        SpawnName = "weapon_deagle",
        Perm = Permission.WPDeagle,
        // desert eagle is a pistol
        Group = 416676503 // pistol/handgun category value
    };
    
    // To add a new weapon, follow this template:
    var myWeaponHash = (uint)GetHashKey("weapon_myweapon");
    CustomWeapons["weapon_myweapon"] = new ValidWeapon
    {
        Hash = myWeaponHash,
        Name = "My Cool Weapon",
        SpawnName = "weapon_myweapon",
        Perm = Permission.WPMyCoolWeapon, // must match what you added in Step 1
        // Set the right group/category (use one of these values):
        Group = 416676503 // pistol/handgun
        // Group = 3337201093 // SMG
        // Group = 1159398588 // LMG
        // Group = 860033945 // shotgun
        // Group = 970310034 // assault rifle
        // Group = 3082541095 // sniper
        // Group = 2725924767 // heavy
        // Group = 3566412244 // melee
        // Group = 1548507267 // throwable
    };
}
```

### Step 3: Add Weapon Display Name
If needed, add an entry for your weapon in the `weaponNames` dictionary:

```csharp
public static readonly Dictionary<string, string> weaponNames = new()
{
    // Add your weapon, e.g.:
    { "weapon_myweapon", "My Cool Weapon" },
    // Don't add it twice! Only add it here if not already in CustomWeapons
};
```

### Step 4: Register Weapon Permission
In `weaponPermissions` dictionary, add your weapon permission mapping:

```csharp
public static readonly Dictionary<string, Permission> weaponPermissions = new()
{
    // Add your weapon, e.g.:
    ["weapon_myweapon"] = Permission.WPMyCoolWeapon,
};
```

## Adding Custom Components

### Step 1: Register Component Names
Add your component names to the `weaponComponentNames` dictionary:

```csharp
private static readonly Dictionary<string, string> weaponComponentNames = new()
{
    // Example for Desert Eagle component:
    ["COMPONENT_DEAGLE_CLIP_01"] = GetLabelText("WCT_CLIP1"),
    
    // Add your components:
    ["COMPONENT_MYWEAPON_CLIP_01"] = GetLabelText("WCT_CLIP1"),
    ["COMPONENT_MYWEAPON_CLIP_02"] = GetLabelText("WCT_CLIP2"),
    ["COMPONENT_MYWEAPON_SCOPE"] = GetLabelText("WCT_SCOPE"),
    // Use appropriate labels from the game, or custom text like:
    // ["COMPONENT_MYWEAPON_THERMAL"] = "Thermal Scope",
};
```

### Step 2: Set up Permissions
Update `permissions.cfg` in your server to include the new weapon permission:

```
add_ace group.admin vMenu.WeaponOptions.WPDeagle allow
add_ace group.admin vMenu.WeaponOptions.WPMyCoolWeapon allow
```

## Weapon Category IDs

Here's a list of weapon category IDs to help you categorize your custom weapons:

| Category | Hash |
|----------|------|
| Pistols/Handguns | 416676503 |
| SMGs | 3337201093 |
| LMGs | 1159398588 |
| Shotguns | 860033945 |
| Assault Rifles | 970310034 |
| Sniper Rifles | 3082541095 |
| Heavy Weapons | 2725924767 |
| Melee Weapons | 3566412244 |
| Throwables | 1548507267 |

## Important Notes

1. **Build and Deploy**: After making changes, rebuild vMenu and copy both DLLs to your server.
2. **No addon.json needed**: This method bypasses the need for entries in the `addons.json` file.
3. **Addon to Direct Conversion**: If you've previously used the addon weapons menu, make sure to remove those entries to avoid duplicates.
4. **Component Naming**: Component names should follow GTA's naming convention: `COMPONENT_[WEAPON]_[PART]_[VARIANT]`.
5. **Troubleshooting**: If your weapon doesn't appear, check that:
   - The hash is correct
   - The permission is properly added and configured
   - The weapon category ID is correct
   - The server is restarted after making changes

## Example Implementation

Here's the complete implementation of the Desert Eagle as an example:

```csharp
// In PermissionsManager.cs - add permission
WPDeagle,

// In ValidWeapon.cs - add custom weapon
var deagleHash = (uint)GetHashKey("weapon_deagle");
CustomWeapons["weapon_deagle"] = new ValidWeapon
{
    Hash = deagleHash,
    Name = "Desert Eagle",
    SpawnName = "weapon_deagle", 
    Perm = Permission.WPDeagle,
    Group = 416676503 // pistol/handgun category
};

// In ValidWeapon.cs - add component
["COMPONENT_DEAGLE_CLIP_01"] = GetLabelText("WCT_CLIP1"),

// In ValidWeapon.cs - add to permissions dictionary
["weapon_deagle"] = Permission.WPDeagle,
```
