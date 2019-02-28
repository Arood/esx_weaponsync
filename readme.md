Copyright (c) 2019 Marcus Olovsson https://github.com/Arood
May not be redistributed without permission.

This script replaces the weapon loadout system in FiveM and ESX. 
Instead of handling the loadout as its own thing, all weapons are
stored as items in the player's inventory. Ammo is also stored as
items.

Before using this, you should modify ESX to allow "silent" removal
of inventory items, or the players will be spammed when they are
shooting. In my version of ESX, I modified the following in 
`es_extended/client/main.lua`:

    RegisterNetEvent('esx:removeInventoryItem')
    AddEventHandler('esx:removeInventoryItem', function(item, count, silent)

      for i=1, #ESX.PlayerData.inventory, 1 do
        if ESX.PlayerData.inventory[i].name == item.name then
          ESX.PlayerData.inventory[i] = item
        end
      end

      if not silent then
        ESX.UI.ShowInventoryItemNotification(false, item, count)
      end

      if ESX.UI.Menu.IsOpen('default', 'es_extended', 'inventory') then
        ESX.ShowInventory()
      end

    end)

Secondly, you must add all weapons you would like to use in your item
database. See the attached `items.sql` for an example.

Lastly, you must modify ESX and all other resources that handles
the weapon loadout to use items instead. Otherwise things will f up.

