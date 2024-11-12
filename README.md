# Disable-Hands-Up-when-downed
This is only a read me for copying the code of line and then put it into the handsup lua 
Disable Hands Up when downed
Put this line of code into qb-smallresources/client/handsup.lua 


local QBCore = exports['qb-core']:GetCoreObject()
local animDict = "missminuteman_1ig_2"
local anim = "handsup_base"
local handsup = false
local disableHandsupControls = {24, 25, 47, 58, 59, 63, 64, 71, 72, 75, 140, 141, 142, 143, 257, 263, 264}

RegisterCommand('hu', function()
    local ped = PlayerPedId()
    if not HasAnimDictLoaded(animDict) then
        RequestAnimDict(animDict)
        while not HasAnimDictLoaded(animDict) do
            Wait(100)
        end
    end
        local PlayerData = QBCore.Functions.GetPlayerData()
    if exports['qb-policejob']:IsHandcuffed() or PlayerData.metadata["isdead"] or PlayerData.metadata["inlaststand"] then
        return
    end
    
    handsup = not handsup
    if handsup then
        TaskPlayAnim(ped, animDict, anim, 2.0, 2.5, -1, 50, 0, false, false, false)
        exports['qb-smallresources']:addDisableControls(disableHandsupControls)
    else
        ClearPedTasks(ped)
        exports['qb-smallresources']:removeDisableControls(disableHandsupControls)
    end
end, false)


RegisterKeyMapping('hu', 'Put your hands up', 'KEYBOARD', 'X')

exports('getHandsup', function() return handsup end)
