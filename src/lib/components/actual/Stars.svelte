<script>
	import {T, useFrame, useThrelte} from '@threlte/core';
	import { Instance, InstancedMesh, useTexture } from '@threlte/extras';
	import {Color, DoubleSide, Euler, Vector3, Quaternion} from 'three';

	let STARS_COUNT = 350;
	let colors = ['#850d8e', '#c759ad', '#ff00b5', '#b90da1', '#f143d5'];
	let stars = [];
	let rot = 1;

	const map = useTexture('textures/star.png');

	const { scene, renderer, camera } = useThrelte();

	function r(min, max) {
		let diff = Math.random() * (max - min);
		return min + diff;
	}

	function resetStar(star) {
		star.pos = new Vector3(r(-200, 200), r(200, 300), r(200, -200));
		star.len = r(1.5, 3);

		star.speed = r(19.5, 42);
		star.rad = r(0.04, 0.07);
		star.color = new Color(colors[Math.floor(Math.random() * colors.length)])
			.convertSRGBToLinear()
			.multiplyScalar(1.3);

		return star;
	}

	function initStar(star) {
		star.pos = new Vector3(r(-200, 200), r(100, 500), r(200, -200));
		star.len = r(1.5, 3);

		star.speed = r(19.5, 42);
		star.rad = r(0.04, 0.07);
		star.color = new Color(colors[Math.floor(Math.random() * colors.length)])
				.convertSRGBToLinear()
				.multiplyScalar(1.3);

		return star;
	}

	for (let i = 0; i < STARS_COUNT; i++) {
		let star = {
			pos: null,
			len: null,
			speed: null,
			color: null
		};

		stars.push(initStar(star));
	}

	useFrame((_, delta) => {
		stars.forEach((star) => {
			star.pos.y -= star.speed * delta;
			if (star.pos.y < -200) resetStar(star);
		});
		stars = stars;
	});

	// function getEulerStar(star) {
	// 	let rot = new Euler().setFromQuaternion(new Quaternion().setFromUnitVectors(new Vector3(0, 0, 1), camera.current.position.clone().sub(star.pos).normalize()));
	// 	return [0, rot.x, Math.PI / 2]
	// }
</script>

{#await map then value}
	<InstancedMesh limit={STARS_COUNT} range={STARS_COUNT}>
		<T.PlaneGeometry args={[3, 0.05]} />
		<T.MeshBasicMaterial side={DoubleSide} alphaMap={value} transparent />

		{#each stars as star}
			<Instance
				position={[star.pos.x, star.pos.y, star.pos.z]}
				scale={[star.len * 5, 10, 10]}
				color={star.color}
				rotation={[0, rot, Math.PI / 2]}
			/>
		{/each}
	</InstancedMesh>
{/await}