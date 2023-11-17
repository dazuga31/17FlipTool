# 17FlipTool
Flip Tool Item, for 17.


[qb]\qb-smallresources\server\consumables.lua:

QBCore.Functions.CreateUseableItem("flipkit", function(source)
    TriggerClientEvent("consumables:client:Useflipkit", source)
end)


[qb]\qb-smallresources\client\consumables.lua


----- Flip Tool

local function IsVehicleUpsideDown(vehicle)
    local roll = GetEntityRoll(vehicle)
    return roll > 75.0 or roll < -75.0
end

RegisterNetEvent('consumables:client:Useflipkit')
AddEventHandler('consumables:client:Useflipkit', function()
    local ped = PlayerPedId()
    local vehicle = QBCore.Functions.GetClosestVehicle()

    if vehicle ~= 0 then
        local vehPos = GetEntityCoords(vehicle)
        local pedPos = GetEntityCoords(ped)
        local distance = GetDistanceBetweenCoords(vehPos.x, vehPos.y, vehPos.z, pedPos.x, pedPos.y, pedPos.z, true)

        if distance <= 2.0 then
            local isFlipped = IsVehicleUpsideDown(vehicle)
            if isFlipped then
                QBCore.Functions.Progressbar("use_flipkit", "Flipping Vehicle", 10000, false, true, {
                    disableMovement = false,
                    disableCarMovement = false,
                    disableMouse = false,
                    disableCombat = true,
                }, {}, {}, {}, function()
                    SetVehicleOnGroundProperly(vehicle)
                    QBCore.Functions.Notify("Vehicle flipped successfully.", 'success')
                end)
            else
                QBCore.Functions.Notify("The vehicle doesn't need to be flipped.", 'error')
            end
        else
            QBCore.Functions.Notify("You need to be closer to the vehicle to use the flip kit.", 'error')
        end
    else
        QBCore.Functions.Notify("No vehicle nearby to flip.", 'error')
    end
end)


[qb]\qb-core\shared\items.lua:

['flipkit'] 							= { ['name'] = 'flipkit',					['label'] = 'Інструмент для перевертання авто', 				['weight'] = 0, 		['type'] = 'item', ['x'] = 4,	['y'] = 3, 		['image'] = 'mechanic_tools.png', 				['unique'] = false, ['useable'] = true, 		['shouldClose'] = true, 	['combinable'] = nil, 	['description'] = 'Повний набір інструментів, необхідний для роботи Механіка' },
