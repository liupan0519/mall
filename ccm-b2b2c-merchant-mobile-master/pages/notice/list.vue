<template>
	<view class="content">
		<view class="announcement-section">
			<view @click="navAnnouncement" class="mix-list-cell b-b">
				<text class="cell-icon yticon icon-dizhi" style="color: #9789f7"></text>
				<text class="cell-tit clamp">官方资讯</text>
				<uni-badge type="error" v-if="announcement.length>0" class="num" :text="announcement.length+''"></uni-badge>
				<text class="cell-tip" v-if="announcement.length>0">{{announcement[0].title}}</text>
				<text class="cell-more yticon icon-you"></text>
			</view>
		</view>
		<view class="notice-section">
			<view @click="navNotice" class="mix-list-cell b-b">
				<text class="cell-icon yticon icon-share" style="color: #5fcda2"></text>
				<text class="cell-tit clamp">活动通知</text>
				<uni-badge type="error" v-if="notice.length>0" class="num" :text="notice.length+''"></uni-badge>
				<text class="cell-tip" v-if="notice.length>0">{{notice[0].title}}</text>
				<text class="cell-more yticon icon-you"></text>
			</view>
		</view>
	</view>
</template>

<script>
	import listCell from '@/components/mix-list-cell';
	import uniBadge from "@/components/uni-badge/uni-badge.vue";
	import {
		mapState,
		mapMutations
	} from 'vuex';
	export default {
		components: {
			listCell,
			uniBadge
		},
		data() {
			return {
				notice: [],
				announcement: [],
			}
		},
		onLoad() {
			this.inquiryArticle();
		},
		computed: {
			...mapState(['hasLogin', 'userInfo', 'footPrint'])
		},
		methods: {
			//查询公告活动
			inquiryArticle: function() {
				let that = this;
				this.$api.request.inquiryArticle({}, function(res) {
					if (res.body.status.statusCode === '0') {
						var articleList = res.body.data.articles;
						var noticeList = [];
						var announcementList = [];
						articleList.forEach(function(val, index) {
							if (val.articleType == '4') {
								announcementList.push(val);
							}
							if (val.articleType == '5') {
								noticeList.push(val);
							}
						})
						that.notice = noticeList;
						that.announcement = announcementList;
					}
				})
			},
			//公告
			navNotice: function() {
				var notice = JSON.stringify(this.notice);
				notice = encodeURIComponent(notice);
				uni.navigateTo({
					url: '/pages/notice/notice?data=' + notice
				});
			}, 
			//活动
			navAnnouncement: function() {
				debugger
				var announcement = JSON.stringify(this.announcement);
				announcement = encodeURIComponent(announcement);
				uni.navigateTo({
					url: '/pages/notice/notice?data=' + announcement
				});
			}
		}
	}
</script>

<style lang='scss'>
	page {
		width: 100%;
		height: 100%;
		background-color: #f8f8f8;
	}

	.content {
		background-color: #fff;
	}

	.top_li {
		background-color: #fff;
		width: 95%;
		margin: auto;
		height: 45px;
		margin: 20px 10px;
	}

	.top_i {
		width: 12%;
		height: inherit;
		float: left;
		margin-right: 5%;
		position: relative;
	}

	image {
		height: 100%;
		width: 100%;
	}

	.top_a {
		height: 45px;
		float: left;
		color: #999999;
		font-size: 15px;
		padding-bottom: 20px;
		border-bottom: 0.5rpx solid #F8F8F8
	}

	.icon .mix-list-cell.b-b:after {
		left: 90upx;
	}

	.mix-list-cell {
		display: flex;
		align-items: baseline;
		padding: 20upx $page-row-spacing;
		line-height: 60upx;
		position: relative;

		&.cell-hover {
			background: #fafafa;
		}

		&.b-b:after {
			left: 30upx;
		}

		.cell-icon {
			align-self: center;
			width: 56upx;
			max-height: 60upx;
			font-size: 38upx;
		}

		.cell-more {
			align-self: center;
			font-size: 30upx;
			color: $font-color-base;
			margin-left: $uni-spacing-row-sm;
		}

		.cell-tit {
			flex: 1;
			font-size: $font-base;
			color: $font-color-dark;
			margin-right: 10upx;

			.num {
				background: #dd524d;
				color: #fff;
				position: absolute;
			}
		}

		.cell-tip {
			text-align: right;
			font-size: $font-sm+2upx;
			color: $font-color-light;
		}
	}

	.msg-section {
		.uni-badge {
			position: absolute;
			top: 2upx;
			left: 50upx;
			background: #dd524d;
			color: #fff;
		}
	}

	.announcement-section {
		.uni-badge {
			position: absolute;
			top: 2upx;
			left: 50upx;
			background: #dd524d;
			color: #fff;
		}
	}

	.notice-section {
		.uni-badge {
			position: absolute;
			top: 2upx;
			left: 50upx;
			background: #dd524d;
			color: #fff;
		}
	}
</style>
