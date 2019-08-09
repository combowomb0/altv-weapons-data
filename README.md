# altv-weapons-data

weapons data with components.
without VARMOD.

## simple usage:
```ts
interface WeaponData {
  hash: number;
  ammo: number;
  components: Array<number>;
}
export const getLoadout = () => {
  let data: WeaponData[] = [];
  const playerPed = alt.Player.local.scriptID;
  Object.entries(weapons).forEach(([weaponName, weaponInfo]) => {
    if (weaponName === 'unarmed') return;
    if (native.hasPedGotWeapon(playerPed, weaponInfo.hash, false)) {
      const hash = weaponInfo.hash;
      const ammo = native.getAmmoInPedWeapon(playerPed, hash);
      const components: WeaponData['components'] = []
      if (weaponInfo.components) {
        weaponInfo.components.forEach(element => {
          if (native.hasPedGotWeaponComponent(playerPed, hash, element)) {
            components.push(element);
          }
        });
      };
      data.push({
        hash: hash,
        ammo: ammo,
        components: components
      });
    }
  });
  alt.log(JSON.stringify(data));
  return data;
};
```